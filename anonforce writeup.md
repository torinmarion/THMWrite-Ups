# TryHackMe Anonforce Writeup

Starting off by running `nmap -p- -T4` to quickly scan the target for all ports gives us only two services hosted:

`port 21 - FTP`

and

`port 22 - SSH`

We don't have any credentials for SSH yet, so I'm going to try an "anonymous" login on the FTP server.

`ftp $IP`
![Screenshot_2023-06-23_18_51_39](https://github.com/torinmarion/THMWrite-Ups/assets/33186777/b9b5b8db-b223-4b25-b978-d2d74ad2020e)
