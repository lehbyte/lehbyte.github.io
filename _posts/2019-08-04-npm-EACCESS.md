---
layout: post
title: npm EACCESS solution ( Linux )
hide_title: false
feature-img: assets/img/pexels/nodejs.png
tags: [Nodejs, EACCESS, npm, web, development]
author-id: lehbyte
---

Suppose you want to install an npm package globally  as in npm install -g git-pending but for whatever reason encounter a permissions problem, EACCESS for example. One of the things you could do to fix that is by running the npm command with sudo privileges, as in; `sudo npm install -g git-pending.`

However if you do this often enough, it becomes a cumbersome and tedious.
You should not have to type in your password every time you want to install an npm package.

Now you can modify your permissions and terminal settings to alleviate the pressure of having to type in your password everyone and then but the problem will persist.

A better solution is to to create a global folder for your packages in your home directory such as `home/user/.npm-global/` then tell npm about it by setting `NPM_CONFIG_PREFIX=~/.npm-global` or just doing `npm config set prefix '~/.npm-global'.`

Lastly you would need to export the bin folder. This would usually be done in your `.bashrc` file or you could append the command in your .profile, source it and relaunch the terminal.

However, if you use zsh like so many of us then you would edit your `.zshrc` instead. Here is an example;

```bash
NPM_CONFIG_PREFIX=~/.npm-global
export PATH=~/.npm-global/bin;$PATH
# export PATH=~/.config/composer/vendor/bin:$PATH
```

The last line is commented out because it is not necessary.

After that, you would source `~/.zshrc`, close the terminal and launch it again. Then you can try to install whatever package you had wanted to originally install; `npm install -g git-pending.`