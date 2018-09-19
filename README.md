# WSO2 Stream Processor Ansible scripts

This repository contains the Ansible scripts for installing and configuring WSO2 Stream Processor.

## Supported Operating Systems

- Ubuntu 16.04 or higher
## Supported Ansible Versions

- Ansible 2.6.2

## Directory Structure
```
.
├── dev
│   ├── group_vars
│   │   └── sp.yml
│   ├── host_vars
│   │   ├── dashboard_1.yml
│   │   ├── editor_1.yml
│   │   ├── manager_1.yml
│   │   └── worker_1.yml
│   └── inventory
├── docs
│   ├── images
│   └── Pattern-1.md
├── files
│   ├── mysql-connector-java-5.1.45-bin.jar
│   └── wso2sp-linux-installer-x64-4.2.0.deb
├── issue_template.md
├── LICENSE
├── pull_request_template.md
├── README.md
├── roles
│   ├── dashboard
│   │   ├── tasks
│   │   │   ├── custom.yml
│   │   │   └── main.yml
│   │   └── templates
│   │       ├── carbon-home
│   │       │   ├── bin
│   │       │   │   └── dashboard.sh.j2
│   │       │   └── conf
│   │       │       └── dashboard
│   │       │           └── deployment.yaml.j2
│   │       └── wso2sp-dashboard.service.j2
│   ├── editor
│   │   ├── tasks
│   │   │   ├── custom.yml
│   │   │   └── main.yml
│   │   └── templates
│   │       ├── carbon-home
│   │       │   ├── bin
│   │       │   │   └── editor.sh.j2
│   │       │   └── conf
│   │       │       └── editor
│   │       │           └── deployment.yaml.j2
│   │       └── wso2sp-editor.service.j2
│   ├── manager
│   │   ├── tasks
│   │   │   ├── custom.yml
│   │   │   └── main.yml
│   │   └── templates
│   │       ├── carbon-home
│   │       │   ├── bin
│   │       │   │   └── manager.sh.j2
│   │       │   └── conf
│   │       │       └── manager
│   │       │           └── deployment.yaml.j2
│   │       └── wso2sp-manager.service.j2
│   └── worker
│       ├── tasks
│       │   ├── custom.yml
│       │   └── main.yml
│       └── templates
│           ├── carbon-home
│           │   ├── bin
│           │   │   └── worker.sh.j2
│           │   └── conf
│           │       └── worker
│           │           └── deployment.yaml.j2
│           └── wso2sp-worker.service.j2
└── site.yml

```

## Packs to be Copied

Copy the following files to `files` directory.

1. [WSO2 Stream Processor 4.2.0 package](https://wso2.com/analytics-and-stream-processing/install/)
2. [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/5.1.html)

## Running WSO2 Stream Processor Ansible scripts

### 1. Run the existing scripts without customization
The existing Ansible scripts contain the configurations to set-up WSO2 Stream Processor. In order to deploy that, you need to replace the `[ip_address]` and `[ssh_user]` given in the `inventory` file under `dev` folder by the IP of the location where you need to host the Stream Processor and the SSH user. An example is given below.
```
[sp]
wso2sp ansible_host=172.28.128.4 ansible_user=vagrant
```

Run the following command to run the scripts.

`ansible-playbook -i dev site.yml`

If you need to alter the configurations given, please change the parameterized values in the yaml files under `group_vars` and `host_vars`.

### 2. Customize the WSO2 Ansible scripts

The templates that are used by the Ansible scripts are in j2 format in-order to enable parameterization.

The `axis2.xml.j2` file is added under `roles/wso2sp/templates/carbon-home/repositoy/conf/axis2/`, in order to enable customizations. You can add any other customizations to `custom.yml` under tasks of each role as well.

#### Step 1
Uncomment the following line in `main.yml` under the role you want to customize.
```
- import_tasks: custom.yml
```

#### Step 2
Add the configurations to the `custom.yml`. A sample is given below.

```
- name: "Copy custom file"
  template:
    src: path/to/example/file/example.xml.j2
    dest: destination/example.xml.j2
  when: "(inventory_hostname in groups['sp'])"
```

Follow the steps mentioned under `docs` directory to customize/create new Ansible scripts and deploy the recommended patterns.
