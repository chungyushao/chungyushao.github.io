---
layout: post
title: "Linux: public and private keys"
excerpt: ""
categories: knownotes
tags: [linux]
comments: true
share: true
author: chungyu
---

> * [Ubuntu documentation](https://help.ubuntu.com/community/SSH/OpenSSH/Keys)







# Key-Based SSH Logins
* SSH can use either "RSA" (Rivest-Shamir-Adleman) or "DSA" ("Digital Signature Algorithm") keys.
* RSA is the only recommended choice for new keys.
* With public key authentication, the authenticating entity has a public key and a private key.
* Each key is a large number with special mathematical properties.
* The private key is kept on the computer you log in from,
* while the public key is stored on the .`ssh/authorized_keys` file on all the computers you want to log in to.

# Mechanism
* When you log in to a computer, the SSH server uses the public key to "lock" messages in a way that can only be "unlocked" by your private key
* Public key authentication is a much better solution than passwords for most people. In fact, if you don't mind leaving a private key unprotected on your hard disk, you can even use keys to do secure automatic log-ins - as part of a network backup, for example. Different SSH programs generate public keys in different ways, but they all generate public keys in a similar format: `<ssh-rsa or ssh-dss> <really long string of nonsense> <username>@<host>`



# Generating RSA Keys

```sh
mkdir ~/.ssh
chmod 700 ~/.ssh
ssh-keygen -t rsa

# Your public key is now available as .ssh/id_rsa.pub in your home folder.
```

# passphrase
* As an extra security measure, most SSH programs store the private key in a passphrase-protected format, so that if your computer is stolen or broken in to, you should have enough time to disable your old public key before they break the passphrase and start using your key.
* An SSH key passphrase is a secondary form of security that gives you a little time when your keys are stolen.
* Your SSH key passphrase is only used to protect your private key from thieves. It's never transmitted over the Internet, and the strength of your key has nothing to do with the strength of your passphrase.
* Note that if you protect your key with a passphrase, then when you type the passphrase to unlock it, your local computer will generally leave the key unlocked for a time. So if you use the key multiple times without logging out of your local account in the meantime, you will probably only have to type the passphrase once.
