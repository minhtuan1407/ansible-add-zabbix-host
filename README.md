Video Guide
--------------
https://drive.google.com/file/d/1IhExgBt2eV3_LGYTgtAHy3x6jIeDzzxA

Jenkins
--------------
![Alt text](./image/jenkins.png?raw=true "Jenkins")


Dependencies
------------
No dependencies

Setup passwordless SSH for Jenkins Ansible
----------------
Use https://github.com/minhtuan1407/jenkins-ssh-copy-id to add SSH key to Jenkins

Or use one of the following commands to add SSH key to Jenkins
```
ssh-copy-id -i ~/.ssh/id_rsa root@12.34.56.789 -p22
```
```
ssh minhtuan1407@12.34.56.789 -i minhtuan1407.pem -p22
```
