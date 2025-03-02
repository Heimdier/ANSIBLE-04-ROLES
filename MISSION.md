# 08-ansible-03-yandex

```shell
подготовил три хоста для clickhouse vector и  lighthouse    `
```
![image](https://github.com/user-attachments/assets/7e895910-48c6-4549-94df-2c1842f72407)

- дополнил playbook плэем с установкой и настройкой LightHouse   [site.yml](https://github.com/Heimdier/ANSIBLE-03-LIGHT/blob/main/playbook/site.yml)  
- подготовил inventory-файл [prod.yml](https://github.com/Heimdier/ANSIBLE-03-LIGHT/blob/main/playbook/inventory/prod.yml)
- запустил линтер ansible-lint site.yml и исправил ошибки

![image](https://github.com/user-attachments/assets/62a21d63-6254-4e8f-bd16-4c4e0b16ffe9)
---
- запустил плэйбук с флагом --check
---
![image](https://github.com/user-attachments/assets/f6f6d826-3961-474e-9e8c-111fb88d6964)
![image](https://github.com/user-attachments/assets/c6d1dd6c-9303-4273-a0dc-f4164dd50a44)
---
- запустил плэйбук с флагом --diff на окружении prod.yml несколько раз
---
![image](https://github.com/user-attachments/assets/fc5ace3e-0801-4faa-a32c-a17615a496d2)
![image](https://github.com/user-attachments/assets/4a549cce-9eee-4273-be3e-ce9241fa5886)
---
- проверка что lighthouse открывается
- --
![image](https://github.com/user-attachments/assets/44846983-9a2a-4159-ad85-79965e5f5858)
---

  
