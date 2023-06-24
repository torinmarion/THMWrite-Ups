# TryHackMe carnage Writeup

Room: https://tryhackme.com/room/c2carnage

## Initial Steps

Open up the provided machine, and doubleclick on the `Wireshark` logo on the desktop. It'll take a moment to load up so be patient.

After it loads, click on `File` in the top left, and navigate to the `/home/ubuntu/Desktop/Analysis/carnage.pcap` and open it.

## Question 1: What was the date and time for the first HTTP connection to the malicious IP?

Simply type `http` into the filter bar at the top, and look for the first line underneath. Then click on the little arrow near `Frame` and the answer is found in `Arrival Time`.

## Question 2: What is the name of the zip file that was downloaded?

This is found in the same packet, underneath the `Info` column.

## Question 3: What was the domain hosting the malicious zip file?

Click the arrow near `Hypertext Transfer Protocol` and the answer is found in the `Host:` row.

## Question 4: Without downloading the file, what is the name of the file in the zip file?

Right-click that same packet, and click on `Follow TCP Stream`. Scroll down to the third chunk of data, and read through that first line for the answer.

## Question 5: What is the name of the webserver of the malicious IP from which the zip file was downloaded?

Follow the TCP stream of the first `HTTP` packet, then look at the `server:` field.

## Question 6: What is the version of the webserver from the previous question?

Follow the TCP stream of the first `HTTP` packet, then look at the `x-powered-by:` field.

## Question 7: Malicious files were downloaded to the victim host from multiple domains. What were the three domains involved with this activity?

Type `tcp.port==443 && (frame.time >= "2021-09-24 16:45:11" && frame.time <= "2021-09-24 16:45:30")` into the filter bar. 

Look for the first `TLSv1.2` protocol packet, underneath `Handshake Protocol: Client Hello` click on `Extension: server_name`. 

Underneath that is `Server Name Indication extension`, the first answer is found in the `Server Name:` row. To find the rest, right click that row, and `Apply as Column`. 

Click that new column, and it should show the other malicious domains.

## Question 8: Which certificate authority issued the SSL certificate to the first domain from the previous question?

Using that first `TLSv1.2` protocol packet from the previous question, follow the TCP stream from it, and read through the output. The answer pops up multiple times.

## Question 9:  What are the two IP addresses of the Cobalt Strike servers? Use VirusTotal (the Community tab) to confirm if IPs are identified as Cobalt Strike C2 servers. (answer format: enter the IP addresses in sequential order) 
Quick google search shows that Cobalt Strike C2 servers commonly use the following ports: `22, 80, 443, 3389, 8080, and 50050`. I'm going to first search for two of the HTTP ports 80/8080 and the high port 50050. `tcp.port==80 or tcp.port==8080 or tcp.port==50050` Click on the `Statistics` tab, and then the `Conversations` tab. Click `Limit to display filter, then click the `IPv4` tab, and sort based on packet count. The two IP addresses you're looking for both have the same first octet.

## Question 10: What is the Host header for the first Cobalt Strike IP address from the previous question?

Filter on the first IP, `ip.src

## Question 11: What is the domain name for the first IP address of the Cobalt Strike server? You may use VirusTotal to confirm if it's the Cobalt Strike server (check the Community tab). 

## Question 12: What is the domain name of the second Cobalt Strike server IP?  You may use VirusTotal to confirm if it's the Cobalt Strike server (check the Community tab). 

## Question 13: What is the domain name of the post-infection traffic?

## Question 14: What are the first eleven characters that the victim host sends out to the malicious domain involved in the post-infection traffic? 


## Question ?: Looks like there was some malicious spam (malspam) activity going on. What was the first MAIL FROM address observed in the traffic? 

Type `smtp.req.parameter contains "FROM"` into the filter bar. Click on the first packet, and click on the arrow near the `Simple Mail Transfer Protocol` section. The one row inside contains the answer.

## Question ?: How many packets were observed for the SMTP traffic?

Click on the `Statistics` tab, then `IPv4 Statistics`, then `IP Protocol Types`. Then in the filter bar below, simply type `smtp`. The answer is found under the `Count` column.






