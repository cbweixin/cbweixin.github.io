title: play-with-webssh
date: 2022-05-31 09:13:29
categories:
- tools
tags:
- ssh
---

# try with pipenv
[webssh](https://github.com/huashengdun/webssh)
## pipenv install virtualenv
```bash
鉂??pipenv --python 3.8.9
Creating a virtualenv for this project...
Pipfile: /Users/xin.wei/.trypackage/Pipfile
Using /Users/xin.wei/.pyenv/versions/3.8.9/bin/python3.8 (3.8.9) to create virtualenv...
鉅??Creating virtual environment...created virtual environment CPython3.8.9.final.0-64 in 560ms
  creator CPython3Posix(dest=/Users/xin.wei/.trypackage/.venv, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/Users/xin.wei/Library/Application Support/virtualenv)
    added seed packages: pip==21.0.1, setuptools==54.1.2, wheel==0.36.2
  activators BashActivator,CShellActivator,FishActivator,PowerShellActivator,PythonActivator,XonshActivator

鉁??Successfully created virtual environment!
Virtualenv location: /Users/xin.wei/.trypackage/.venv
Creating a Pipfile for this project...
鉂??pipenv install webssh
Installing webssh...
Adding webssh to Pipfile's [packages]...
鉁??Installation Succeeded
Pipfile.lock not found, creating...
Locking [dev-packages] dependencies...
Locking [packages] dependencies...
Building requirements...
Resolving dependencies...
鉁??Success!
Updated Pipfile.lock (788955)!
Installing dependencies from Pipfile.lock (788955)...
  馃悕   鈻夆枆鈻夆枆鈻夆枆鈻夆枆鈻夆枆鈻夆枆鈻夆枆鈻夆枆鈻夆枆鈻夆枆鈻夆枆鈻夆枆鈻夆枆鈻夆枆鈻夆枆鈻夆枆 0/0 鈥??00:00:00
To activate this project's virtualenv, run pipenv shell.
Alternatively, run a command inside the virtualenv with pipenv run.
鉂??la
total 40
drwxr-xr-x    6 xin.wei  staff   192B May 30 20:11 .
drwxr-xr-x+ 134 xin.wei  staff   4.2K May 30 20:11 ..
drwxr-xr-x    8 xin.wei  staff   256B May 30 20:10 .venv
-rw-r--r--    1 xin.wei  staff   151B May 30 20:10 Pipfile
-rw-r--r--    1 xin.wei  staff    14K May 30 20:11 Pipfile.lock
drwxr-xr-x    4 xin.wei  staff   128B May 30 20:01 webssh
~/.trypackage 鉂??
```
try to run wssh

```bash
鉂??wssh
zsh: command not found: wssh

鉂??pipenv shell
Launching subshell in virtual environment...
 . /Users/xin.wei/.trypackage/.venv/bin/activate
鉂?? . /Users/xin.wei/.trypackage/.venv/bin/activate
鉂??wssh
[I 220530 20:12:57 settings:125] WarningPolicy
[I 220530 20:12:57 main:38] Listening on :8888 (http)
[I 220530 20:13:32 web:2239] 200 GET / (127.0.0.1) 5.00ms
[I 220530 20:13:32 web:2239] 200 GET /static/css/bootstrap.min.css (127.0.0.1) 9.44ms
[I 220530 20:13:32 web:2239] 200 GET /static/css/xterm.min.css (127.0.0.1) 1.43ms
[I 220530 20:13:32 web:2239] 200 GET /static/css/fullscreen.min.css (127.0.0.1) 2.12ms
[I 220530 20:13:32 web:2239] 200 GET /static/js/jquery.min.js (127.0.0.1) 3.14ms
[I 220530 20:13:32 web:2239] 200 GET /static/js/popper.min.js (127.0.0.1) 3.66ms
[I 220530 20:13:32 web:2239] 200 GET /static/js/bootstrap.min.js (127.0.0.1) 4.43ms
[I 220530 20:13:32 web:2239] 200 GET /static/js/xterm.min.js (127.0.0.1) 6.86ms
[I 220530 20:13:32 web:2239] 200 GET /static/js/main.js (127.0.0.1) 1.35ms
[I 220530 20:13:32 web:2239] 200 GET /static/js/xterm-addon-fit.min.js (127.0.0.1) 1.62ms
[I 220530 20:13:33 web:2239] 200 GET /static/img/favicon.png (127.0.0.1) 0.93ms
```
![webssh](https://user-images.githubusercontent.com/1326906/171222311-39a497ab-7ef7-457a-bd99-b0f135dc5e01.png)

looks good.
