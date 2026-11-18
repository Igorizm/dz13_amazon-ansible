# AWS EC2 Provisioning & Java App Deployment (Ansible)

Единый Ansible Playbook (`aws_deploy.yml`) для создания инфраструктуры в AWS EC2, сборки Java-приложения на сборочной ноде и его деплоя на продовый сервер.

## Архитектура и логика работы
1. **Provisioning (`localhost`):** Создает два EC2-инстанса (`java-build-node` и `java-prod-node`) с помощью модуля `amazon.aws.ec2_instance`, ожидает готовности SSH и добавляет их IP в динамический инвентарь (`add_host`).
2. **Build (`build_servers`):** Устанавливает OpenJDK 17 и Maven, клонирует Java-репозиторий, собирает `.jar` и скачивает артефакт на управляющую ноду (`fetch`).
3. **Deploy (`prod_servers`):** Устанавливает OpenJDK 17 JRE, копирует `.jar` на продовую ноду (`copy`) и запускает приложение.

## Подготовка и запуск в AWS
```bash
# 1. Установка зависимостей
pip install boto3 botocore
ansible-galaxy collection install -r requirements.yml

# 2. Передача ключей доступа AWS
export AWS_ACCESS_KEY_ID="your_access_key"
export AWS_SECRET_ACCESS_KEY="your_secret_key"
export AWS_DEFAULT_REGION="us-east-1"

# 3. Запуск плейбука
ansible-playbook aws_deploy.yml
```
