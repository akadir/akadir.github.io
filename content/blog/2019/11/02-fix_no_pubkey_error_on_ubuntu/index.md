---
title: 'Fix GPG Error: Missing NO_PUBKEY error on Ubuntu'
date: "2019-11-15T22:45:32.169Z"
description: 'Solution to "GPG Error: Missing NO_PUBKEY" error on Ubuntu'
---

While I was trying to execute sudo apt-get update on my ubuntu, I saw an error it says something like this:

```
W: GPG error: [some-website-address] xenial InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY [some-16-characters-long-key]
```

After some research I fixed problem by finding missed key from <a href="http://keyserver.ubuntu.com/" target="_blank">http://keyserver.ubuntu.com/</a>, importing it to a file named as key1 and adding it by executing 

```
$ sudo apt-key add key1
```

command.

---

Detailed instructions can be found here: 
<a href="http://opensourceforgeeks.blogspot.com/2013/04/w-gpg-error-httpppalaunchpadnet-precise.html" target="_blank">http://opensourceforgeeks.blogspot.com/2013/04/w-gpg-error-httpppalaunchpadnet-precise.html</a>