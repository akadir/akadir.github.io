---
title: Using git behind proxy on windows
date: "2019-11-15T22:40:32.169Z"
description: It can be getting hard to work behind corporate proxy sometimes, even when you need to clone random github repository to your computer, you may need to make some additional configurations.
---

In this post we will see yet another proxy configuration.

You can make git proxy configurations using `git config http.proxy` command but as you know, this command requires password and it's stored without encryption in .gitconfig file and we don't want this.

Using following list of commands we can configure git to use the standard windows Credential Manager to store credentials.

```sh
git config --global http.proxy http://user@proxy.corporate.com:8080
git config --global https.proxy http://user@proxy.corporate.com:8080
git config http.sslVerify false
git config https.sslVerify false
git config --global credential.helper wincred</code></pre>
```

---

references and further readings:

<a href="https://stackoverflow.com/questions/22799825" target="_blank">- https://stackoverflow.com/questions/22799825</a>

<a href="https://stackoverflow.com/questions/38333752" target="_blank">- https://stackoverflow.com/questions/38333752</a>

<a href="https://gist.github.com/ankurk91/f0b26f1c30d0d6d3ca4e" target="_blank">- git remember password</a>
                
<a href="https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage" target="_blank">- https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage</a><br>
