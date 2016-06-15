Pre-requisites
--------------

Requirements (on Ansible host that executes module)
---------------------------------------------------

* Install python-boto on your Ansible host either through EPEL or pip,

   yum install python-boto
   pip install boto

* Create AWS_ACCESS_KEY and AWS_SECRET_ACCESS_KEY environment variables (either export in a shell or place in your ~/.bashrc file)

   export AWS_ACCESS_KEY_ID='YOUR_AWS_API_KEY' && export AWS_SECRET_ACCESS_KEY='YOUR_AWS_API_SECRET_KEY'

* Add a local inventory to /etc/ansible/hosts file,

   [local]
   localhost

Provision an AWS Instance with Ansible Playbook
-----------------------------------------------

1). Create dynamic inventory file through AWS EC2 External Inventory Script, 

    * Download https://raw.github.com/ansible/ansible/devel/contrib/inventory/ec2.py script to "/etc/ansible" and chmod +x it

2). Also, need to copy https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/ec2.ini to "/etc/ansible" directory. 

3). To successfully make an API call to AWS, you will need to configure Boto (the Python interface to AWS) and export the two environment variables:

    export ANSIBLE_HOSTS=/etc/ansible/ec2.py && export EC2_INI_PATH=/etc/ansible/ec2.ini 

4). Using an SSH agent is the best way to authenticate with end nodes, as this alleviates the need to copy the .pem files around. To add an agent,

    ssh-agent bash && ssh-add ~/.ssh/keypair.pem 

Provision an AWS Instance with Ansible ad hoc method
----------------------------------------------------

$ ansible localhost -m ec2 -a "image=ami-d44b4286 ec2_region=ap-southeast-1 instance_type=m3.medium count=1 keypair=ans_mukul group=default wait=yes" -c local
