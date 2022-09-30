# interview-challenge-terraform
Interview Challenge to provision resources using Terraform

# Objective

1. Create a Python hello world app using Flask
1. Create the resources on OCI as using Terraform
1. Configure and deploy the Flask application using Ansible

# Python

1. Install python dependencies flask and gunicorn
1. Under python directory, modify `app.py` and create python hello world application using flask
1. Generate list of python dependencies for your code and update requirements.txt
1. Test code by running: (app will be running on port 5000)
    - gunicorn -w 4 -b 0.0.0.0:5000 app:app
    - navigate browser to http://\<ip\>:5000


# Terraform

1. Refer file `Steps to Access Oracle Cloud.md` to ensure you can login to Oracle Cloud
1. Create the resources as shown in the pdf file `demo-stack-terraform.pdf`
1. Use the below mentioned variables in your terraform code:
    - Compartment ID : `ocid1.compartment.oc1..aaaaaaaau2d5tlzxsvkpz3iuzhuo4uemcn7kzmkv6fhjqbpxdzpg5ijz4tqq`
    - Image ID : `ocid1.image.oc1.iad.aaaaaaaab3w3vzjenuyy3idksenczspj77wz74o7unpxid6xr7zmsyi7u47q`
    - Shape : `VM.Standard.E4.Flex`
    - Availability Domain : `KGRc:US-ASHBURN-AD-1`
1. Modify public compute instance metadata to add your ssh public key and also add public key below for grading purposes
    > ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC65bld2dmUibITLdPl9GQpHkFyHLOp8VwwyiPUT9qYOOopyW2i/EfsZeINsZSBXKpoS4dpfr9zXbR7M4QJHomhUhd9Wbpoo7fi1wfCUYYP7pL4w74ZZDVKLMoXdBvYsg1udR//efxyOlEyLd6R/Td4nSAjhwalLf9uE4uRX2+eKxPDFyY/AGALT7qiHsqp7tNQ0OvuL0CIN/z+wxBVr2zN+HFp9XPfF9cF31xBFQiRYtXHo2c0brgsJ2zNNiAKet8GOmY0djel9VEemnviRmEsVrgW+Q3upA02BtNNQZt50/mgfNLtY3aaRdB8OZ+yr9YLGQKW8hvhKOu/rbzgUYs1 rbhagat@AJTV3VGQF2.local
1. Create `output.tf` to output public_ip of compute instance


# Ansible

Before beginning ansible section, checkin python code to repo
1. Under ansible directory, modify playbook.yml playbook to perform following on the compute instance:
    1. Install missing python dependencies
    1. Install hello world app python dependencies using ../python/requirements.txt
        - set executable parameter to use pip3
    1. copy files/helloworld.service to /etc/systemd/system
        - make sure files are owned by root:root
        - verify path to repo in helloworld.service is correct. Modify if necessary
    1. enable and start helloworld systemd service
    1. create firewalld rules to enable access to app from public ip addresses. Keep in mind app will be running on port 5000.
1. test code by running:
    - ansible-playbook -i \<ip\>, playbook.yml --private-key ~/.ssh/id_rsa -u opc

# Comments/implemention details/thought process
Feel free to add any comments or details here that would be useful when grading your solution. If you face any issues and you need to make an assumptions or any other step to proceed, please document them.


# Grading Process

1. Under terraform directory, run terraform to create infrastructure
    - terraform init
    - terraform apply
1. Under ansible directory, run ansible-playbook to install hello world app, create firewalld rules, and start app
    - ansible-playbook -i <compute_ip>, playbook.yml --private-key ~/.ssh/id_rsa -u opc
1. Navigate browser to http://\<public_ip\>:5000 to show hello world app is running
1. Reboot compute instance
1. Verify app starts automatically and refresh browser
1. SSH into compute instance to show public_key is working
1. Ping VM in private subnet B to check if connectivity is setup correctly
1. Ping VM in private subnet A should fail since no route exists