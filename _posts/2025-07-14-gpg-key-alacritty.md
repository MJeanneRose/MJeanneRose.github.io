---
layout: post
title: "Creating a GPG key and fixing terminal signing in Alacritty"
date: 2025-07-14
author: prout
tags: gpg, alacritty, pgp, secret, key, terminal, ioctl, github, commit, sign
---

I've recently set up GPG commit signing for Git, and ran into a few annying issue when using Alacritty as my terminal.
Here's a step-by-step log of what I did, what broke, and how I fixed it.

## Step 1 : Creating the GPG Key

```bash
gpg --full-generate-key
```

* Key type: ECC -> Curve 25519
* Usage: Signing + Authentication
* Expiry: 3 years
* UID: Name and emails
* Passphrase: yes

To list the key's ID :
```bash
gpg --list-secret-keys --keyid-format LONG
```

## Step 2 : Exporting the key for GitHub

```bash
gpg --armor --export mail
```

Full block must be pasted as it is, do not add or remove blank line.

## Step 3 : Tellig Git to use this key

```bash
git config --global user.signingkey KeyID
```

Commits can be signed automatically with 

```bash
git config --global commit.gpgsign true
```

# Signing failed in Alacritty 

I got the following error while trying to commit

```bash
git commit -S -m "commit message"
gpg: signing failed: Inappropriate ioctl for device
gpg: [stdin]: clear-sign failed: Inappropriate ioctl for device
gpg: signing failed: Operation cancelled
```

Reason : 

Pinentry (the program asking for my secret's key passphrase), was trying to use an interface that doesn't work in non-GUI session like a terminal.

You just have to install pinentry for dumb terminal

```bash apt install pinentry-tty </code>

Tell gpg-agent to use another version of pinentry

```bash
echo "pinentry-program /usr/bin/pinentry-tty" >> ~/.gnupg/gpg-agent.conf
```

Restart the agent

```bash
gpgconf -K gpg-agent
gpgconf --launch gpg-agent
```

Set the TTY env var for GPG

```bash
export GPG_TTY=$(tty)
```

Optionnaly, you can make it permanent with

```bash
echo "export GPG_TTY=$(tty)" >> ~/.bashrc
```

Finally you can test your gpg installation with :

```bash
echo "test" | gpg --clearsign
```

