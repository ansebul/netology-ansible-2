# Домашнее задание к занятию "08.02 Работа с Playbook"

1. Приготовьте свой собственный inventory файл `prod.yml`.

   > Файл подготовлен. [Посмотреть](./playbook/inventory/prod.yml)

2. ( --> 4)
3. ( --> 4)
4. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает kibana.
       При создании tasks рекомендую использовать модули: `get_url`, `template`, `unarchive`, `file`.
       Tasks должны: скачать нужной версии дистрибутив, выполнить распаковку в выбранную директорию, сгенерировать конфигурацию с параметрами.

   > playbook дописан, task'и тоже. [Посмотреть](./playbook/site.yml#L73)

5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.

   > Устранил ошибки и предупреждения   

<details><summary>Посмотреть вывод</summary>

```yml
abs@len:~/devops/_DEVOPS-15/homeworks/my_repos/netology-ansible-2/playbook$ ansible-lint site.yml
WARNING  Overriding detected file kind 'yaml' with 'playbook' for given positional argument: site.yml
WARNING  Listing 8 violation(s) that are fatal
risky-file-permissions: File permissions unset or incorrect
site.yml:9 Task/Handler: Upload .tar.gz file containing binaries from local storage

risky-file-permissions: File permissions unset or incorrect
site.yml:16 Task/Handler: Ensure installation dir exists

risky-file-permissions: File permissions unset or incorrect
site.yml:32 Task/Handler: Export environment variables

risky-file-permissions: File permissions unset or incorrect
site.yml:52 Task/Handler: Create directrory for Elasticsearch

risky-file-permissions: File permissions unset or incorrect
site.yml:67 Task/Handler: Set environment Elastic

risky-file-permissions: File permissions unset or incorrect
site.yml:87 Task/Handler: Create directrory for Kibana

risky-file-permissions: File permissions unset or incorrect
site.yml:102 Task/Handler: Set environment Kibana

yaml: too many blank lines (1 > 0) (empty-lines)
site.yml:108

You can skip specific rules or tags by adding them to your configuration file:
# .ansible-lint
warn_list:  # or 'skip_list' to silence them completely
  - experimental  # all rules tagged as experimental
  - yaml  # Violations reported by yamllint

Finished with 1 failure(s), 7 warning(s) on 1 files.
```

</details>

6. Попробуйте запустить playbook на этом окружении с флагом `--check`.

   > Получили ошибку и перешли к п.7 ( в режиме --check не вносятся изменения в систему, много не проверишь...)
```yml
TASK [Extract java in the installation directory] *****************************************************************************************************************************************************************************
fatal: [elastik]: FAILED! => {"changed": false, "msg": "dest '/opt/jdk/11.0.16.1' must be an existing dir"}
fatal: [kibana]: FAILED! => {"changed": false, "msg": "dest '/opt/jdk/11.0.16.1' must be an existing dir"}
```

7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. 

```bash
$ ansible-playbook -i ./inventory/prod.yml site.yml --diff
```

<details><summary>Посмотреть вывод</summary>

```yml
[WARNING]: Found both group and host with same name: kibana

PLAY [Install Java] ***********************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ********************************************************************************************************************************************************************************************************
ok: [elastik]
ok: [kibana]

TASK [Set facts for Java 11 vars] *********************************************************************************************************************************************************************************************
ok: [elastik]
ok: [kibana]

TASK [Upload .tar.gz file containing binaries from local storage] *************************************************************************************************************************************************************
diff skipped: source file size is greater than 104448
changed: [kibana]
diff skipped: source file size is greater than 104448
changed: [elastik]

TASK [Ensure installation dir exists] *****************************************************************************************************************************************************************************************
--- before
+++ after
@@ -1,4 +1,4 @@
 {
     "path": "/opt/jdk/11.0.16.1",
-    "state": "absent"
+    "state": "directory"
 }

changed: [kibana]
--- before
+++ after
@@ -1,4 +1,4 @@
 {
     "path": "/opt/jdk/11.0.16.1",
-    "state": "absent"
+    "state": "directory"
 }

changed: [elastik]

TASK [Extract java in the installation directory] *****************************************************************************************************************************************************************************
changed: [kibana]
changed: [elastik]

TASK [Export environment variables] *******************************************************************************************************************************************************************************************
--- before
+++ after: /home/abs/.ansible/tmp/ansible-local-114762550cnw4p/tmp4nf01_76/jdk.sh.j2
@@ -0,0 +1,5 @@
+# Warning: This file is Ansible Managed, manual changes will be overwritten on next playbook run.
+#!/usr/bin/env bash
+
+export JAVA_HOME=/opt/jdk/11.0.16.1
+export PATH=$PATH:$JAVA_HOME/bin
\ No newline at end of file

changed: [kibana]
--- before
+++ after: /home/abs/.ansible/tmp/ansible-local-114762550cnw4p/tmpt7ex2y1u/jdk.sh.j2
@@ -0,0 +1,5 @@
+# Warning: This file is Ansible Managed, manual changes will be overwritten on next playbook run.
+#!/usr/bin/env bash
+
+export JAVA_HOME=/opt/jdk/11.0.16.1
+export PATH=$PATH:$JAVA_HOME/bin
\ No newline at end of file

changed: [elastik]

PLAY [Install Elasticsearch] **************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ********************************************************************************************************************************************************************************************************
ok: [elastik]

TASK [Upload tar.gz Elasticsearch from remote URL] ****************************************************************************************************************************************************************************
changed: [elastik]

TASK [Create directrory for Elasticsearch] ************************************************************************************************************************************************************************************
--- before
+++ after
@@ -1,4 +1,4 @@
 {
     "path": "/opt/elastic/7.10.1",
-    "state": "absent"
+    "state": "directory"
 }

changed: [elastik]

TASK [Extract Elasticsearch in the installation directory] ********************************************************************************************************************************************************************
changed: [elastik]

TASK [Set environment Elastic] ************************************************************************************************************************************************************************************************
--- before
+++ after: /home/abs/.ansible/tmp/ansible-local-114762550cnw4p/tmp27rd_mxr/elk.sh.j2
@@ -0,0 +1,5 @@
+# Warning: This file is Ansible Managed, manual changes will be overwritten on next playbook run.
+#!/usr/bin/env bash
+
+export ES_HOME=/opt/elastic/7.10.1
+export PATH=$PATH:$ES_HOME/bin
\ No newline at end of file

changed: [elastik]

PLAY [Install Kibana] *********************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ********************************************************************************************************************************************************************************************************
ok: [kibana]

TASK [Upload tar.gz Kibana from remote URL] ***********************************************************************************************************************************************************************************
changed: [kibana]

TASK [Create directrory for Kibana] *******************************************************************************************************************************************************************************************
--- before
+++ after
@@ -1,4 +1,4 @@
 {
     "path": "/opt/kibana/8.4.1",
-    "state": "absent"
+    "state": "directory"
 }

changed: [kibana]

TASK [Extract Kibana in the installation directory] ***************************************************************************************************************************************************************************
changed: [kibana]

TASK [Set environment Kibana] *************************************************************************************************************************************************************************************************
--- before
+++ after: /home/abs/.ansible/tmp/ansible-local-114762550cnw4p/tmplb7qwx94/kbn.sh.j2
@@ -0,0 +1,5 @@
+# Warning: This file is Ansible Managed, manual changes will be overwritten on next playbook run.
+#!/usr/bin/env bash
+
+export KIBANA_HOME=/opt/kibana/8.4.1
+export PATH=$PATH:$KIBANA_HOME/bin
\ No newline at end of file

changed: [kibana]

PLAY RECAP ********************************************************************************************************************************************************************************************************************
elastik                    : ok=11   changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
kibana                     : ok=11   changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

</details>
   
8.  Убедитесь, что изменения на системе произведены.

   > Изменения произведены

9.  Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.

   > Видим `changed=0`, изменений никаких не внесено,  идемпотентен

<details><summary>Посмотреть вывод</summary>   

```yml
[WARNING]: Found both group and host with same name: kibana

PLAY [Install Java] ***********************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ********************************************************************************************************************************************************************************************************
ok: [elastik]
ok: [kibana]

TASK [Set facts for Java 11 vars] *********************************************************************************************************************************************************************************************
ok: [elastik]
ok: [kibana]

TASK [Upload .tar.gz file containing binaries from local storage] *************************************************************************************************************************************************************
ok: [kibana]
ok: [elastik]

TASK [Ensure installation dir exists] *****************************************************************************************************************************************************************************************
ok: [kibana]
ok: [elastik]

TASK [Extract java in the installation directory] *****************************************************************************************************************************************************************************
skipping: [elastik]
skipping: [kibana]

TASK [Export environment variables] *******************************************************************************************************************************************************************************************
ok: [elastik]
ok: [kibana]

PLAY [Install Elasticsearch] **************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ********************************************************************************************************************************************************************************************************
ok: [elastik]

TASK [Upload tar.gz Elasticsearch from remote URL] ****************************************************************************************************************************************************************************
ok: [elastik]

TASK [Create directrory for Elasticsearch] ************************************************************************************************************************************************************************************
ok: [elastik]

TASK [Extract Elasticsearch in the installation directory] ********************************************************************************************************************************************************************
skipping: [elastik]

TASK [Set environment Elastic] ************************************************************************************************************************************************************************************************
ok: [elastik]

PLAY [Install Kibana] *********************************************************************************************************************************************************************************************************

TASK [Gathering Facts] ********************************************************************************************************************************************************************************************************
ok: [kibana]

TASK [Upload tar.gz Kibana from remote URL] ***********************************************************************************************************************************************************************************
ok: [kibana]

TASK [Create directrory for Kibana] *******************************************************************************************************************************************************************************************
ok: [kibana]

TASK [Extract Kibana in the installation directory] ***************************************************************************************************************************************************************************
skipping: [kibana]

TASK [Set environment Kibana] *************************************************************************************************************************************************************************************************
ok: [kibana]

PLAY RECAP ********************************************************************************************************************************************************************************************************************
elastik                    : ok=9    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
kibana                     : ok=9    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0 
```

</details>

10. Подготовьте README.md файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.

   > Файл подготовлен. [Посмотреть](./playbook/README.md)

11. Готовый playbook выложите в свой репозиторий, в ответ предоставьте ссылку на него.

   > [Перейти](./playbook/)
