#### 1. Создал в старой версии playbook файл [requirements.yml](https://github.com/Heimdier/ANSIBLE-04-ROLES/blob/main/playbook/requirements.yml) и заполнил его содержимым:  
```shell
---
  - src: git@github.com:AlexeySetevoi/ansible-clickhouse.git
    scm: git
    version: "1.13"
    name: clickhouse 
```

#### 2. При помощи ansible-galaxy скачал себе эту роль.  
![image](https://github.com/user-attachments/assets/3e6d06e9-5e42-4f70-bb08-84d10919353e)

#### 3. Создал каталоги для ролей:   
`ansible-galaxy role init vector-role`  
`ansible-galaxy role init lighthouse-role`  

![image](https://github.com/user-attachments/assets/85463b5d-2495-41a0-8f5d-eb72560cc561)



#### 4. Из старого playbook перенес в созданные роли: tasks, vars, template, handlers. И разнес переменные между vars и default:   
- в default оставил:
`vector_version: "0.45.0"`
`lighthouse_nginx_port: 8888`
- в vars остальные переменные:
```
vector_config_dir: "/etc/vector/"
vector_config_file: "vector.toml"
```
```
lighthouse_code_src: "https://github.com/VKCOM/lighthouse.git"
lighthouse_data_dir: "/etc/lighthouse"
lighthouse_packages:
  - git
  - nginx
lighthouse_nginx_conf: "lighthouse.conf"
```
#### 5. запустил основной плэйбук с применением ролей:
```shell
maha@mahavm:~/ansible-galaxy/ansible-04-roles$ ansible-playbook site.yml -i ./inventory/prod.yml

PLAY [Install Clickhouse] ********************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : Include OS Family Specific Variables] *************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : include_tasks] ************************************************************************************************************
included: /home/maha/ansible-galaxy/ansible-04-roles/roles/clickhouse/tasks/precheck.yml for clickhouse-01

TASK [clickhouse : Requirements check | Checking sse4_2 support] *****************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : Requirements check | Not supported distribution && release] ***************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : include_tasks] ************************************************************************************************************
included: /home/maha/ansible-galaxy/ansible-04-roles/roles/clickhouse/tasks/params.yml for clickhouse-01

TASK [clickhouse : Set clickhouse_service_enable] ********************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : Set clickhouse_service_ensure] ********************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : include_tasks] ************************************************************************************************************
included: /home/maha/ansible-galaxy/ansible-04-roles/roles/clickhouse/tasks/install/apt.yml for clickhouse-01

TASK [clickhouse : Install by APT | Apt-key add repo key] ************************************************************************************
changed: [clickhouse-01]

TASK [clickhouse : Install by APT | Remove old repo] *****************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : Install by APT | Repo installation] ***************************************************************************************
changed: [clickhouse-01]

TASK [clickhouse : Install by APT | Package installation] ************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : Install by APT | Package installation] ************************************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : Hold specified version during APT upgrade | Package installation] *********************************************************
changed: [clickhouse-01] => (item=clickhouse-client)
changed: [clickhouse-01] => (item=clickhouse-server)
changed: [clickhouse-01] => (item=clickhouse-common-static)

TASK [clickhouse : include_tasks] ************************************************************************************************************
included: /home/maha/ansible-galaxy/ansible-04-roles/roles/clickhouse/tasks/configure/sys.yml for clickhouse-01

TASK [clickhouse : Check clickhouse config, data and logs] ***********************************************************************************
ok: [clickhouse-01] => (item=/var/log/clickhouse-server)
changed: [clickhouse-01] => (item=/etc/clickhouse-server)
changed: [clickhouse-01] => (item=/var/lib/clickhouse/tmp/)
changed: [clickhouse-01] => (item=/var/lib/clickhouse/)

TASK [clickhouse : Config | Create config.d folder] ******************************************************************************************
changed: [clickhouse-01]

TASK [clickhouse : Config | Create users.d folder] *******************************************************************************************
changed: [clickhouse-01]

TASK [clickhouse : Config | Generate system config] ******************************************************************************************
changed: [clickhouse-01]

TASK [clickhouse : Config | Generate users config] *******************************************************************************************
changed: [clickhouse-01]

TASK [clickhouse : Config | Generate remote_servers config] **********************************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : Config | Generate macros config] ******************************************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : Config | Generate zookeeper servers config] *******************************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : Config | Fix interserver_http_port and intersever_https_port collision] ***************************************************
skipping: [clickhouse-01]

TASK [clickhouse : Notify Handlers Now] ******************************************************************************************************

RUNNING HANDLER [clickhouse : Restart Clickhouse Service] ************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : include_tasks] ************************************************************************************************************
included: /home/maha/ansible-galaxy/ansible-04-roles/roles/clickhouse/tasks/service.yml for clickhouse-01

TASK [clickhouse : Ensure clickhouse-server.service is enabled: True and state: restarted] ***************************************************
changed: [clickhouse-01]

TASK [clickhouse : Wait for Clickhouse Server to Become Ready] *******************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : include_tasks] ************************************************************************************************************
included: /home/maha/ansible-galaxy/ansible-04-roles/roles/clickhouse/tasks/configure/db.yml for clickhouse-01

TASK [clickhouse : Set ClickHose Connection String] ******************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : Gather list of existing databases] ****************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : Config | Delete database config] ******************************************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : Config | Create database config] ******************************************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : include_tasks] ************************************************************************************************************
included: /home/maha/ansible-galaxy/ansible-04-roles/roles/clickhouse/tasks/configure/dict.yml for clickhouse-01

TASK [clickhouse : Config | Generate dictionary config] **************************************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : include_tasks] ************************************************************************************************************
skipping: [clickhouse-01]

PLAY [Install Vector] ************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************
ok: [vector-01]

TASK [vector : Get vector distrib] ***********************************************************************************************************
ok: [vector-01]

TASK [vector : Install vector packages] ******************************************************************************************************
ok: [vector-01]

TASK [vector : Flush handlers to restart vector] *********************************************************************************************

TASK [vector : Vector | check the directory exists] ******************************************************************************************
ok: [vector-01]

TASK [vector : Configure Vector | Template config] *******************************************************************************************
ok: [vector-01]

PLAY [Install Lighthouse] ********************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************
ok: [lighthouse-01]

TASK [lighthouse : Lighthouse. Pre-install nginx & git client] *******************************************************************************
ok: [lighthouse-01]

TASK [lighthouse : Lighthouse. Clone source code by git client] ******************************************************************************
ok: [lighthouse-01]

TASK [lighthouse : Lighthouse | Apply nginx config] ******************************************************************************************
ok: [lighthouse-01]

TASK [lighthouse : Lighthouse | Prepare Lighthouse config] ***********************************************************************************
ok: [lighthouse-01]

PLAY RECAP ***********************************************************************************************************************************
clickhouse-01              : ok=27   changed=9    unreachable=0    failed=0    skipped=10   rescued=0    ignored=0   
lighthouse-01              : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
vector-01                  : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

maha@mahavm:~/ansible-galaxy/ansible-04-roles$ 

```
#### все завершилось успешно!

#### 6. Ссылки на полученные roles в моем репозитории и основной плэйбук:  

[vector-role](https://github.com/Heimdier/ANSIBLE-VECTOR-ROLE)    
[lighthouse-role](https://github.com/Heimdier/ANSIBLE-LIGHTHOUSE-ROLE)      
[main playbook](https://github.com/Heimdier/ANSIBLE-04-ROLES/blob/main/playbook/site.yml)



