---
description: 220117 - 如何正确地在 Linux 中部署应用。
---
# 如何正确地在 Linux 中部署应用

在做项目的过程中发现有时候团队对应用部署的方法并不规范，所以萌生了写一篇博客的想法，也是对自己的理解的一点总结。

假设我们生成了一个名字叫 demo 的 binary，其启动的方法为最简单的 `./demo --config=<PATH_TO_CONFIG_FILE>`，我们应该如何将其以最规范的方式部署到 Linux 呢？

如果我们希望所有用户都能使用这个 binary，我们应该将 demo 这个 binary 放入 `/usr/local/bin` 这个文件夹中 - `mv demo /usr/local/bin`。这里插一嘴， `/usr/bin` 这个路径一般包含属于操作形同的可执行二进制文件，一般该操作系统的 package manager，如 apt, yum，会将程序安装在这个路径；而 `/usr/local/bin` 这个路径则一般用于安装不属于操作系统的二进制文件，这样可以防止系统程序被用户安装的程序覆盖导致整个系统崩溃，一般像 `configure; make; make install` 这样的安装命令会默认把程序安装到这里。当然，如果你只想某个登录用户能够调用这个 binary，也可以在用户目录下创建一个 bin 文件夹，比如在 CentOS `/user/damon/home/bin` 创建文件夹，然后将这个路径通过 `~/.bash_profile` 或者 `~/.profile` 暴露给 PATH，再将可执行的二进制文件放入这个文件夹中。

下一步我们需要在 `/etc` 中为 demo 程序的配置文件创建一个文件夹，例如 `sudo mkdir -p /etc/deom`。在 Linux 中惯例上将 `/etc` 路径视为放置配置文件的地方，所以我们会将配置文件安装到这个文件夹 - `sudo mv config.yml /etc/demo`。

为系统安全考虑，我们正常不应该在非必要的情况下使用 root 启动程序，所以我们会为 demo 程序创建一个用户 - `sudo useradd -rs /bin/false demo`，`-r` 表示该用户是一个系统用户，`-s /bin/false` 指定该用户无 shell，因此这个用户只能用于启动 demo 程序，不能作为一个用户进行登录。

最后我们要保证新创建的用户有足够的权限能够调用 demo 程序和读取配置文件 - `sudo chown demo:demo /usr/local/bin/demo; sudo chown -R demo:demo /etc/blackbox/*`。

接下来有一个可选步骤，取决于我们是否想将程序用 systemd (当然 supervisor 也是可以用的，这里就不细说了) 对该进程进行管理。如果回答是 yes 的话，我们首先应该在 `/usr/lib/systemd/system/` 创建这个服务的配置文件，

```
[Unit]
Description=demo service
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=demo
Group=demo
ExecStart=/usr/local/bin/demo \
  --config.file=/etc/demo/demo.yml

Restart=always

[Install]
WantedBy=multi-user.target
```

不同的需求会导致这个配置文件也有所不同，这里也不多说了。再将这个系统服务配置文件用软链接连到 `/etc/systemd/system` - `sudo ln -s /usr/lib/systemd/system/demo.service /etc/systemd/system/demo.service`。最后我们可以启动这个服务并设置开机自动启动 - `sudo systemctl start demo; sudo systemctl enable demo`。