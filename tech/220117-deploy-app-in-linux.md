---
description: How to Deploy Applications in Linux Properly
---
# How to Deploy Applications in Linux Properly

In the course of working on the project, I found that sometimes the team's approach to application deployment is not standardized, so I had the idea of writing a blog, which is also a little summary of my understanding.

Let's say we generate a binary called demo, which is started with the simplest method `. /demo --config=<PATH_TO_CONFIG_FILE>`, how should we deploy it to Linux in the most standardized way?

If we want the binary to be available to all users, we should put the demo binary in the `/usr/local/bin` folder - `mv demo /usr/local/bin`. As an aside, the `/usr/bin` path generally contains executable binaries that belong to the operating system, and the package manager for that operating system, such as apt, yum, will install the program in this path; whereas the `/usr/local/bin` path is generally used to install binaries that don't belong to the operating system, which prevents system programs from being installed by the user, and prevents system programs from being installed by the user. The `/usr/local/bin` path is generally used for installing non-OS binaries, so as to prevent system programs from being overwritten by user-installed programs and causing the system to crash, and is where installation commands like `configure; make; make install` install programs by default. Of course, if you only want a logged-in user to be able to call the binary, you can create a bin folder in the user's directory, e.g., CentOS `/user/damon/home/bin`, and expose the path to the PATH via `~/.bash_profile` or `~/.profile`. PATH, and then place the executable binary in this folder.

Next we need to create a folder in `/etc` for the demo program's configuration files, e.g. `sudo mkdir -p /etc/deom`. By convention in Linux, the `/etc` path is considered to be where the configuration files are placed, so we will install the configuration files into this folder - `sudo mv config.yml /etc/demo`.

For system security reasons, we shouldn't normally use root to start the program when it's not necessary, so we'll create a user for the demo program - ``sudo useradd -rs /bin/false demo``, where ``-r`'' means that the user is a system user, and ``-s /bin/false`'' specifies that the user doesn't have a shell, and so This user can only be used to start the demo program, not to log in as a user.

Finally, we need to make sure that the newly created user has enough privileges to be able to invoke the demo program and read configuration files - `sudo chown demo:demo /usr/local/bin/demo; sudo chown -R demo:demo /etc/blackbox/*`.

There is an optional step depending on whether or not we want the program to be managed by systemd (of course, supervisor is also available, so I won't go into details here). If the answer is yes, we should first create a configuration file for the service in `/usr/lib/systemd/system/`.

```
[Unit]
Description=demo service
Wants=network-online.target
After=network-online.target

[Service] Description=demo service Wants=network-online.target After=network-online.target
After=network-online.target [Service] Type=simple
User=demo
Group=demo
ExecStart=/usr/local/bin/demo \
  --config.file=/etc/demo/demo.yml

Restart=always

[Install]
WantedBy=multi-user.target
```

Different requirements will cause this configuration file to be different as well, so I won't go into that here. Connect this system service configuration file with a soft link to `/etc/systemd/system` - `sudo ln -s /usr/lib/systemd/system/demo.service /etc/systemd/system/demo.service`. Finally we can start this service and set it to start automatically on boot - `sudo systemctl start demo; sudo systemctl enable demo`.