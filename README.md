# Mundia Flux7 Trial Project

## Project Info

Variables file: `vars.yml` is used to hold repeated variables throughout the playbooks. As Boto3 is used for interaction with AWS, security credentials are store here. Since this is sensitive information, the file is encrypted by the `ansible-vault` command. Thus, the `--ask-vault-pass` argument needs to be passed when running any playbooks in the project.

### Workflow:

- A security group is created based on values from the `vars` file. SG allows traffic on port 22 (SSH) and 80 (HTTP).
- 3 `t2.micro` instances are launched into different availability zones (also from the `vars` file) and tagged.
- The local host file is updated after the launch of each instance with the public IP of the instance.
- Once instances are launched, the `webservers` host group is targeted with installation of Apache and PHP. The basic `index.php` file is uploaded to the application path.
- To confirm success of the operation, a debug command is run to print out the contents of the application directory.
- As we do not want Apache to restart on _all_ changes to the application, a service handler is used to only restart Apache is the `.ini` file is changed.

### Instructions to run:

- Install Ansible on the control machine and modify the local ansible.cfg file as needed.
- Confirm that you have a AWS EC2 key-pair in the region you want to launch instances in.
- Populate the `vars.yml` file with the values appropriate for your desired setup.
  - _Note that you will have to add in the IDs of existing subnets._
- Encrypt the vars.yml file using the `ansible-vault` command.
- Run the `all-playbooks.yml` file with the `ask-vault-pass` argument.
- After the playbook is run, confirm successful deployment by browsing to each instance's public IP.