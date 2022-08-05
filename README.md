# vncuseradd
adds new LOGIN(s) with VNC capabilities

* needs TigerVNC Server installed with config at /etc/tigervnc/vncserver.users
* 
VNC passwords are set to 'password' by default.

```
Usage: vncuseradd [OPTION] -p PASSWORD LOGIN [LOGIN]...

Options:
  -a            make the LOGIN(s) an admin account
  -p PASSWORD   password of the new account(s)
                for more than one LOGIN, PASSWORD will apply to all. 
                this input method is *NOT SECURE*. have user change ASAP.
  -h            display this help message and exit
  ```
  
  
