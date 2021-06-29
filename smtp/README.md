# SMTP Simple Test Server for DevOpsLab1 Prometheus Grafana

This server is just for testing alerting messages in the DevOpsLab Prometheus Grafana.
It must not be used in a production environment as is.

## Installation

The server is installed in the full docker-compose instalation of the DevOpsLab of Prometheus-Grafana
but there is a stand-alone docker-compose.yml in this directory if you want to play just with this smtp server

```bash
$ docker-compose up -d
$
```

## Testing the installation

### Checking up and running

After installed, the smtp-relay process will appear in the docker process list

```bash
$ docker ps | grep "smtp"
$
```

### Checking network connectivity to outside email systems

Execute the next command in the DevOpsLab/smtp directory to ensure there is no network problems in your installations.
The CURL command will try to send a simple test message (stored in the "email-contents.txt" file) to
the user you define in the "mail-rcpt" parameter (please, ensure you put a valid email address that you will have access to verify correct reception).
In the user parameter (-u "username:password) you can put any user you want (as if not validated in this example as insecure

```bash
$ curl --connect-timeout 15 -v --insecure "smtp://localhost:25" -u "username:password"
\ --mail-from "sender@example.com" --mail-rcpt "destination@example.com"
\ -T email-test.txt --ssl
```

If success, you will see something similar to this

```
 % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   
  Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 25 (#0)
< 220 7ebd77b05553 ESMTP Exim 4.92 Sat, 06 Feb 2021 15:07:13 +0000
> EHLO email-test.txt
< 250-7ebd77b05553 Hello your-hostname.local [172.21.0.1]
< 250-SIZE 52428800
< 250-8BITMIME
< 250-PIPELINING
< 250-CHUNKING
< 250-PRDR
< 250 HELP
> MAIL FROM:<sender@example.com> SIZE=159
< 250 OK
> RCPT TO:<destination@example.com>
< 250 Accepted
> DATA
< 354 Enter message, ending with "." on a line by itself
} [159 bytes data]
* We are completely uploaded and fine
< 250 OK id=1l8PB3-00004R-NR
100   159    0     0  100   159      0   1347 --:--:-- --:--:-- --:--:--  1347
* Connection #0 to host localhost left intact
```
