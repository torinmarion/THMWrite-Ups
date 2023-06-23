# TryHackMe Anonforce Writeup

Starting off by running `nmap -p- -T4` to quickly scan the target for all ports gives us only two services hosted:

`port 21 - FTP`

and

`port 22 - SSH`

We don't have any credentials for SSH yet, so I'm going to try an "anonymous" login on the FTP server.

`ftp $IP`

Type in "anonymous" for the username, and just hit enter when it asks for a password.

![248427123-b9b5b8db-b223-4b25-b978-d2d74ad2020e](https://github.com/torinmarion/THMWrite-Ups/assets/33186777/eacb5b06-fc9c-4ecd-a2cb-431da8be3cd4)

Most of these directories looks pretty standard, however there is a "notread" directory that pops out.
