---
title: Password encryption in settings.xml via maven
date: "2019-10-15T22:40:32.169Z"
description: Encrypt passwords used in settings.xml to prevent plaintext password usage
---

Let's say you are making your developments behind corporate proxy and you need to configure your maven settings according to this proxy configurations but at the same time you don't want to type your password as plain-text in your settings.xml because of the security considerations.

In order to solve this problem first you need to generate master password to store in settings-security.xml as following structure:

```xml
<settingsSecurity>
    <master>{rsB56BJcqoEHZqEZ0R1VR4TIspmODx1Ln8/PVvsgaGw=}</master>
</settingsSecurity>
```

To generate this master password you need to run this command:

```sh
$ mvn -emp mypassword
{rsB56BJcqoEHZqEZ0R1VR4TIspmODx1Ln8/PVvsgaGw=}
```

After running the command you will get your master password.

And then you need to encrypt your user password. To do this simply running below command will be enough: (Let's say your password is: loremIpsum)

```sh
$ mvn -ep loremIpsum
{7SX3V+1VIcEHBN9GukeB+dwz5e5GLHHzXe2xinPsjLE=}
```

This command will generate your encrypted password. You can now use your encrypted password in your settings.xml file:

```xml
...
<proxies>
    <proxy>
    <id>example-proxy</id>
    <active>true</active>
    <protocol>http</protocol>
    <host>proxy.yourcorporate.com</host>
    <port>8080</port>
    <username>yourUsername</username>
    <password>{7SX3V+1VIcEHBN9GukeB+dwz5e5GLHHzXe2xinPsjLE=}</password>
    <nonProxyHosts>localhost|127.0.0.1</nonProxyHosts>
    </proxy>
</proxies>
...
```

<br>

---

references and further readings:

<a href="https://blog.sonatype.com/2009/10/maven-tips-and-tricks-encrypting-passwords/" target="_blank">- Maven Tips and Tricks: Encrypting Passwords</a><br>
<a href="http://maven.apache.org/guides/mini/guide-encryption.html" target="_blank">- Password Encryption</a><br>