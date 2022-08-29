# Installing working openaps

- That repository installs openaps on ```da70837952034a9f23794eda7751701434d34508``` commit of ```dev``` branch.
- Original openaps: https://github.com/openaps/oref0

## Before you start:
- **Save your preferences.json**
- Optional: save your autotune
- **Login into root user by ```sudo su -```**
- **Go to rig's home directory: ```cd /root```**

## 1) Remove two openaps folders:
1) ```rm -r src```
2) ```rm -r myopenaps```

## 2) Paste following commands:

1) ```curl https://raw.githubusercontent.com/workingopenaps/oref0/dev/bin/openaps-install.sh > /tmp/openaps-install.sh```
2) ```bash /tmp/openaps-install.sh```. When script will ask you ```Press Enter to run oref0-setup``` **press** **```CTRL-C```**

3) Paste this command: ```cd && ~/src/oref0/bin/oref0-setup.sh -dev```

4) Apply your preferences.json
5) Apply your autotune folder or just run ```cd /root/myopenaps && mkdir autotune``` to enable SMB

6) Check after 20 minutes, if you have **/root/test_data** directory.

## Optional: set autoremoving test_data folder
1) Go to crontab editing: ```crontab -e```
2) Paste ```* * * * * cd /root && rm -r test_data```
3) Save
4) Check the change by ```crontab -l```
