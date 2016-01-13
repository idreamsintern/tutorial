# Change local on AWS EC2 Ubuntu
```
sudo locale-gen zh_TW zh_TW.UTF-8
sudo locale-gen
sudo vim /etc/default/locale
```
Then `sudo vim /etc/default/locale`, and change the file to
```
LANG="zh_TW.UTF-8"
LC_CTYPE="zh_TW.UTF-8"
LC_MESSAGES="zh_TW.UTF-8"
LC_TIME="zh_TW.UTF-8"
```
Then log out and log in

