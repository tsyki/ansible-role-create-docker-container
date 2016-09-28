Create docker container
=========

* Install docker
* Create image that enable ssh
* Run container

After you run the playbook , you can connect to the container with the following command.

    ssh  -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no docker@docker_host -p 50001

Requirements
------------

none

Role Variables
--------------

    pull_image_name: centos
    pull_image_tag: 7
    dockerfile_dir: /tmp/
    build_image_name: "{{pull_image_name}}/ssh_enabled"
    port_mapping: -p 50002:80 -p 50001:22
    container_name: ssh_enabled
    
    base_image_name: "{{pull_image_name}}:{{pull_image_tag}}"
    maintainer_name:
    maintainer_mail_address:
    root_password: admin
    work_user_name: docker
    work_user_password: docker


Dependencies
------------

none

Example Playbook
----------------

    - hosts: servers
      become: yes
      vars_prompt:
        - name: root_password
          prompt: input root_password
        - name: worker_password
          prompt: input worker_password
      roles:
        - { role: tsyki.create-docker-container, maintainer_name: YourName, maintainer_mail_address: youraddress@example.com}

License
-------

BSD

Author Information
------------------

Toshiyuki Imaizumi
