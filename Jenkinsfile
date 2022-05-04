pipeline {
    agent { label 'built-in' }
    options {
        ansiColor('vga')
    }
    parameters {
        string(name: 'ip_server', defaultValue: '', description: '')
        string(name: 'port_server', defaultValue: '10050', description: '')
        string(name: 'zabbix_server', defaultValue: '103.127.206.135', description: '')
        string(name: 'customer_name', defaultValue: '', description: '')
        string(name: 'port_ssh', defaultValue: '22', description: '')
        string(name: 'server_order_number', defaultValue: '01', description: '')
        choice(name: 'system_name', choices: ['CloudPBX', 'SBC', 'PitelPBX', 'Autocall', 'PitelCallCenter', 'BillingAST', 'BillingFS', 'ELK', 'Database', 'WebServer', 'LAB'], description: '')
        choice(name: 'zabbix_agent_group', choices: ['PRODUCTION', 'INTERNAL'], description: '')
    }
    stages {
        stage ("Install zabbix agent") {
            steps {
                ansiblePlaybook (
                    playbook: '${WORKSPACE}/ansible-install-zabbix-agent.yml',
                    inventory: '${WORKSPACE}/hosts_all_server',
                    tags: 'install-zabbix-agent',
                    extraVars: [
                        ip_server: [value: '${ip_server}', hidden: false],
                        port_server: [value: '${port_server}', hidden: false],
                        zabbix_server: [value: '${zabbix_server}', hidden: false]
                    ]
                )
            }
        }
        stage ("Copy config file") {
            steps {
                ansiblePlaybook (
                    playbook: '${WORKSPACE}/ansible-install-zabbix-agent.yml',
                    inventory: '${WORKSPACE}/hosts_all_server',
                    tags: 'copy-config-file',
                    extraVars: [
                        ip_server: [value: '${ip_server}', hidden: false],
                        port_server: [value: '${port_server}', hidden: false],
                        zabbix_server: [value: '${zabbix_server}', hidden: false],
                        customer_name: [value: '${customer_name}', hidden: false],
                        system_name: [value: '${system_name}', hidden: false],
                        server_order_number: [value: '${server_order_number}', hidden: false],
                        system_name: [value: '${system_name}', hidden: false]
                    ]
                )
            }
        }
        stage ("Add zabbix host") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'tel4vn-zabbix-account', passwordVariable: 'zabbix_admin_password', usernameVariable: 'zabbix_admin_user')]) {
                    ansiblePlaybook (
                        playbook: '${WORKSPACE}/ansible-add-zabbix-agent.yml',
                        inventory: '${WORKSPACE}/hosts_all_server',
                        tags: 'add-host-${system_name}',
                        extraVars: [
                            customer_name: [value: '${customer_name}', hidden: false],
                            system_name: [value: '${system_name}', hidden: false],
                            server_order_number: [value: '${server_order_number}', hidden: false],
                            zabbix_agent_group: [value: '${zabbix_agent_group}', hidden: false],
                            system_name: [value: '${system_name}', hidden: false],
                            ip_server: [value: '${ip_server}', hidden: false],
                            port_server: [value: '${port_server}', hidden: false],
                            zabbix_admin_user: [value: '${zabbix_admin_user}', hidden: true],
                            zabbix_admin_password: [value: '${zabbix_admin_password}', hidden: true]
                        ]
                    )
                }
            }
        }
        stage ("Add zabbix action") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'tel4vn-zabbix-account', passwordVariable: 'zabbix_admin_password', usernameVariable: 'zabbix_admin_user')]) {
                    ansiblePlaybook (
                        playbook: '${WORKSPACE}/ansible-add-zabbix-agent.yml',
                        inventory: '${WORKSPACE}/hosts_all_server',
                        tags: 'add-action-${system_name}',
                        extraVars: [
                            customer_name: [value: '${customer_name}', hidden: false],
                            system_name: [value: '${system_name}', hidden: false],
                            server_order_number: [value: '${server_order_number}', hidden: false],
                            zabbix_agent_group: [value: '${zabbix_agent_group}', hidden: false],
                            system_name: [value: '${system_name}', hidden: false],
                            port_ssh: [value: '${port_ssh}', hidden: false],
                            ip_server: [value: '${ip_server}', hidden: false],
                            port_server: [value: '${port_server}', hidden: false],
                            zabbix_admin_user: [value: '${zabbix_admin_user}', hidden: true],
                            zabbix_admin_password: [value: '${zabbix_admin_password}', hidden: true]
                        ]
                    )
                }
            }
        }
    }
    post {
        always {
            // notifyEvents message: '''Tuan is testing
            // Build <a href="$PROJECT_URL">$PROJECT_NAME</a>
            // Build Number <a href="$BUILD_URL">$BUILD_NUMBER</a> result with status: <b>$BUILD_STATUS</b>
            // <a href="$BUILD_URL/console">Build log</a> on host ${ip_server}''',
            //     token: 'zqMYpf7aCt0Wl3T_IMdsh-LOUzf7_G8T'

            // emailext subject: 'Jenkins ${BUILD_STATUS} [#${BUILD_NUMBER}] - ${PROJECT_NAME}',
            // body: '''Build <a href="$PROJECT_URL">$PROJECT_NAME</a> <br>
            // Build Number <a href="$BUILD_URL">$BUILD_NUMBER</a> result with status: <b>$BUILD_STATUS</b> <br>
            // <a href="$BUILD_URL/console">Build log</a> on host ${ip_server}''',
            // to: 'tech@tel4vn.com',
            // attachLog: true

            telegramSend(message: '''Build [$PROJECT_NAME]($PROJECT_URL) \nBuild Number [$BUILD_NUMBER]($BUILD_URL) result with status: *$BUILD_STATUS* \n[Build log]($BUILD_URL/console) on host ${ip_server}''',
            chatId:1284662325)
        }
    }
}
