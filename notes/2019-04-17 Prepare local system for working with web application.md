## 2019-04-17 Prepare local system for working with web application

### Used Ansible to install Java 8 in WSL Debian environment

- I have an Ansible playbook for installing openjdk8 on Debian:  
  https://github.com/tmcphillips/ansible-playbooks/blob/master/debian/jdk8.yml

- On circe-win10 started Debian in WSL environment and confirmed that Java is not yet installed:

    ```console
    tmcphill@circe-win10:~/GitRepos$ java -version
    -bash: java: command not found
    ```
- Activated the Python2 virtual environment where Ansible is installed:

    ```console
    tmcphill@circe-win10:~/GitRepos/ansible-playbooks$ . .venv-ansible-playbooks/bin/activate
    ```
- Ran the playbook for installing openjdk8 on Debian:

	```console
	(.venv-ansible-playbooks) tmcphill@circe-win10:~/GitRepos/ansible-playbooks/debian$ ansible-playbook -vK jdk8.yml
	Using /etc/ansible/ansible.cfg as config file
	SUDO password:
	/etc/ansible/hosts did not meet host_list requirements, check plugin documentation if this is unexpected
	/etc/ansible/hosts did not meet script requirements, check plugin documentation if this is unexpected
	/etc/ansible/hosts did not meet yaml requirements, check plugin documentation if this is unexpected
	/etc/ansible/hosts did not meet ini requirements, check plugin documentation if this is unexpected
	/etc/ansible/hosts did not meet auto requirements, check plugin documentation if this is unexpected
	 [WARNING]: Unable to parse /etc/ansible/hosts as an inventory source

	 [WARNING]: No inventory was parsed, only implicit localhost is available

	 [WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

	PLAY [install OpenJDK 8 JDK]
	
	TASK [Gathering Facts]
	ok: [127.0.0.1]
	
	TASK [install openjdk-8-jdk]
	ok: [127.0.0.1] => {"cache_update_time": 1555640135, "cache_updated": false, "changed": false}

	PLAY RECAP 
	127.0.0.1 : ok=2    changed=0    unreachable=0    failed=0
	```
- Exited virtual environment and confirmed that Java is now installed:

    ```console
    (.venv-ansible-playbooks) tmcphill@circe-win10:~/GitRepos/ansible-playbooks/debian$ deactivate
    
    tmcphill@circe-win10:~/GitRepos/ansible-playbooks/debian$ java -version
    openjdk version "1.8.0_212"
    OpenJDK Runtime Environment (build 1.8.0_212-8u212-b01-1~deb9u1-b01)
    OpenJDK 64-Bit Server VM (build 25.212-b01, mixed mode)
    ```

