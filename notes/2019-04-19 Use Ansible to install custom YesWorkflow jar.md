## 2019-04-19 Use Ansible to install custom YesWorkflow jar

### Developed a new ansible playbook that installs the custom jar

- First commit of new playbook to tmcphillips/ansible-playbooks repo: [yw_gonzaga.yml](https://github.com/tmcphillips/ansible-playbooks/blob/8d823bde4a8d09d83aaffa36fedfea34a215181a/debian/yw_gonzaga.yml)

- The playbook:

    ```yaml
    - name: install yw jar developed by Gonzaga University students
      hosts: 127.0.0.1
      connection: local
      become: no
      tasks:
    
      - name: create a directory for holding yesworkflow jar files
        file:
            path: ~/.yw/jars
            state: directory
            mode: 0755
    
      - name: download and install custom yw jar file
        get_url:
            url: https://github.com/aniehuser/senior-design-group10/raw/master/yesworkflow-4_12_2019.jar
            dest: ~/.yw/jars/
            mode: 0644
    
      - name: update the symbolic link yw-gonzaga.jar to point to the newly download jar
        file:
            src: ~/.yw/jars/yesworkflow-4_12_2019.jar
            dest: ~/.yw/jars/yw-gonzaga.jar
            state: link
    
      - name: create and set contents of an initializer script to be run by bash at login
        copy:
            dest:  ~/.bashrc.d/yw-gonzaga.sh
            content: |
                # define an alias for running the yw_gonzaga jar
                alias yw-gonzaga='java -jar ~/.yw/jars/yw-gonzaga.jar'
            mode: 0644
    
    ```

- Ran playbook:

    ```console
    (.venv-ansible-playbooks) tmcphill@circe-win10:~/GitRepos/ansible-playbooks/debian$ ansible-playbook yw_gonzaga.yml
     [WARNING]: Unable to parse /etc/ansible/hosts as an inventory source
    
     [WARNING]: No inventory was parsed, only implicit localhost is available
    
     [WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'
    
    
    PLAY [install yw jar developed by Gonzaga University students] *****************************************************************************************************************************
    
    TASK [Gathering Facts] *********************************************************************************************************************************************************************
    ok: [127.0.0.1]
    
    TASK [create a directory for holding yesworkflow jar files] ********************************************************************************************************************************
    changed: [127.0.0.1]
    
    TASK [download and install custom yw jar file] *********************************************************************************************************************************************
    changed: [127.0.0.1]
    
    TASK [update the symbolic link yw-gonzaga.jar to point to the newly download jar] **********************************************************************************************************
    changed: [127.0.0.1]
    
    TASK [create and set contents of an initializer script to be run by bash at login] *********************************************************************************************************
    ok: [127.0.0.1]
    
    PLAY RECAP *********************************************************************************************************************************************************************************
    127.0.0.1                  : ok=5    changed=3    unreachable=0    failed=0
    ```
- Confirmed that jar file was downloaded to new ~/.yw/jars directory and a symbolic link created representing the current version of the jar:

    ```console
    (.venv-ansible-playbooks) tmcphill@circe-win10:~/GitRepos/ansible-playbooks/debian$ ls -alR ~/.yw
    /home/tmcphill/.yw:
    total 0
    drwxr-xr-x 1 tmcphill tmcphill 4096 Apr 19 18:49 .
    drwxr-xr-x 1 tmcphill tmcphill 4096 Apr 19 18:49 ..
    drwxr-xr-x 1 tmcphill tmcphill 4096 Apr 19 18:49 jars
    
    /home/tmcphill/.yw/jars:
    total 30912
    drwxr-xr-x 1 tmcphill tmcphill     4096 Apr 19 18:49 .
    drwxr-xr-x 1 tmcphill tmcphill     4096 Apr 19 18:49 ..
    -rw-r--r-- 1 tmcphill tmcphill 31472464 Apr 19 18:49 yesworkflow-4_12_2019.jar
    lrwxrwxrwx 1 tmcphill tmcphill       49 Apr 19 18:49 yw-gonzaga.jar -> /home/tmcphill/.yw/jars/yesworkflow-4_12_2019.jar
    ```

- Confirmed that bash customization script that defines alias was created:

    ```console
    (.venv-ansible-playbooks) tmcphill@circe-win10:~/GitRepos/ansible-playbooks/debian$ cat ~/.bashrc.d/yw-gonzaga.sh
    # define an alias for running the yw_gonzaga jar
    alias yw-gonzaga='java -jar ~/.yw/jars/yw-gonzaga.jar'
    ```
- Started new bash shell and confirmed that new alias works correctly:

    ```console
    tmcphill@circe-win10:~$ yw-gonzaga -v
    -----------------------------------------------------------------------------
    YesWorkflow 0.2.1.2-104 (branchmaster, commit 2a48569)
    -----------------------------------------------------------------------------
    Remote repo: https://github.com/yesworkflow-org/yw-prototypes.git
    Git branch: master
    Last commit: 2a4856919dec85177a0dedfe5279724ff8c49ced
    Commit time: 12.04.2019 @ 14:34:42 PDT
    Most recent tag: v0.2.0
    Commits since tag: 104
    Builder name: Anthony Niehuser
    Builder email: 33637158+aniehuser@users.noreply.github.com
    Build host: Anthonys-MacBook-Pro.local
    Build platform: Mac OS X 10.14.2 (x86_64)
    Build Java VM: Java HotSpot(TM) 64-Bit Server VM (Oracle Corporation)
    Build Java version: JDK 1.8.0_131
    Build time: 12.04.2019 @ 14:35:19 PDT
    ```
- I can now quickly install the custom YW jar file and alias definition in other Debian (hopefully also any Ubuntu) environments.

