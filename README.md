# Ansible Project on provisioning EC2 machines on loop and shutting down 

This project aims to provision multiple ec2 instances through ansible loops. Create passwordless authentication and configure worker nodes to shutdown 

1. At first create IAM role in AWS to allow control node to access AWS resources (EC2 full access)
2. Use ansible galaxy collection to install aws on control node 
3. Install boto3 module for control node to interact with AWS resources
4. Setup vault 
Since we are dealing with sensitive information like AWS access and secret key for control node to authorize and use EC2 resources in AWS we cannot directly hardcode AWS access and secret keys. 
5. Create a base64 encoded password file called vault.pass using openssl

```
openssl rand -base64 2048 > vault.pass
```
6. Then create a ansible vault file called pass.yml which will be protected with password file

```
ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass
```
Provide the access key and secret key created 

7. Create a ec2 create yaml file 
Since the control node is going to talk to aws use hosts as localhost and connection also local and start with task

8. Understand loops and idempotency nature of ansible - give unique name to each instance

9. Execute the playbook with vault password file to create instances

10. Next for passwordless authentication for worker nodes 

```
ssh-copy-id -f "-o IdentityFile <PATH TO PEM FILE>" ubuntu@<INSTANCE-PUBLIC-IP>
```
11. Next time you can easilt log with ssh ubuntu@public_ip, avoiding pem key 

12. Create invcentory file with all worker nodes username@ip added

13. Create ec2_shotdown yaml 

Provide when condition to define which family of instance to terminate 

14. Execute the yaml with inventory file and playbook and password

So this project aims to provision aws resource ec2 through ansible and from control node it provisions multiple EC2 using ansible loops. Before that control node(my laptop) creates a passwordless authentication with worker nodes> 
