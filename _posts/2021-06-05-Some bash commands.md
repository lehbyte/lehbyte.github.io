---
layout: post
title: Some useful bash commands
color: 'green'
# feature-img: "assets/img/features/green_terminal.jpg"
thumbnail: "assets/img/features/green_terminal.jpg"
# image: "assets/img/features/green_terminal.png"
tags: [bash,commands,linux,terminal]
author-id: lehbyte
---

Bash is one of the most powerful  tools for doing all sorts of things in linux.<br>
You can automate pretty much anything in bash and bellow are some useful commands. <br>
Here are snippets of common bash commands I use.

### Getting root permissions on a file inside of vi

`:w !sudo tee %`

Choose [L] to reload file as confirmation
Or simply use `visudo`

### logging into mysql as root

`mysql -u root -p`

### changing password

`mysqladmin -u root -p password newpass`

### secure installation

`mysql_secure_installation`

### change a normal user password 
`mysqladmin -u user-name -p password newpass`

### Creating a new user
```sh
start mysql as root
mysql> CREATE USER yourname@localhost;
GRANT ALL PRIVILEGES ON mydb.* To yourname@localhost IDENTIFIED BY 'yourpassword'
```

### Reset rails database

`bin/rake db:reset db:migrate`

### destroy database and create it then migrate current schema

`bin/rake db:drop db:create db:migrate`

### Duplicate current line in vscode

`CTRL + SHIFT + NUMPAD2`
