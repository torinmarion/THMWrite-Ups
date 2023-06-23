# TryHackMe Anonforce Writeup

Room: https://tryhackme.com/room/bsidesgtanonforce

## Initial Scanning

Starting off by running `nmap -p- -T4` to quickly scan the target for all ports gives us only two services hosted:

`port 21 - FTP`

and

`port 22 - SSH`

## Anonymous FTP Login

We don't have any credentials for SSH yet, so I'm going to try an "anonymous" login on the FTP server.

`ftp $IP`

Type in "anonymous" for the username, and just hit enter when it asks for a password.

![248427123-b9b5b8db-b223-4b25-b978-d2d74ad2020e](https://github.com/torinmarion/THMWrite-Ups/assets/33186777/eacb5b06-fc9c-4ecd-a2cb-431da8be3cd4)

Most of these directories looks pretty standard, however there is a "notread" directory that pops out.

![Screenshot_2023-06-23_19_02_32](https://github.com/torinmarion/THMWrite-Ups/assets/33186777/ccb5afa9-a992-49a9-a8af-abf2963478fa)

Inside of the "notread" directory are two files. "backup.pgp" and "private.asc", using `mget *` will attempt to download all files in the current FTP directory.

## PGP Hash Cracking

A `.asc` file is a armored ASCII file used by PGP. For our purposes, it will be used to decrypt the `.pgp` file.

First we use `gpg2john private.asc > privatehash`

then

`john --wordlist=/usr/share/wordlists/rockyou.txt privatehash`

The prior command should crack it pretty quickly, giving up a password we can use.

`gpg --import private.asc` which will then prompt for the password we just got.

Once that's imported, we then use it to open up `backup.pgp`

`gpg --decrypt backup.pgp`

## Cracking /etc/shadow Hashes

The output of this is the target's `/etc/shadow` file, which contains some password hashes. Specifially a `$6$` for root, and `$1$` for the user melodias. `$6$` being a SHA-512 hash, and `$1$` being a MD5 hash.

Paste that output into a file, and then we'll run `john` on it.

`john hashes --wordlist=/usr/share/wordlists/rockyou.txt`

## Root SSH Login

Which then gives us the root creds.

`ssh root@$IP` 

Enter the creds, and we've rooted the box. From here just cat the `root.txt` file for the root hash, and navigate to the `/home/melodias` directory for the `user.txt` file that contains the user hash.

