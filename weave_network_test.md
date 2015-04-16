# Weave / Docker Network Performance Tests
I was looking for some network performance numbers for Weave, but was not very successful. So, I decided to test it myself. 
Thanks to its great documentation and ease of usage, I had very little trouble setting it up for some testing fun. 
 
## The Set Up
* Two bare-metal servers (HP ProLiant DL360p Gen8)
* OS: CentOS 7.0. Kernel: 3.10.0-123.13.2.el7.x86_64
* Docker: 1.5.0, build a8a31ef/1.5.0
* iperf version: 2.0.5 (08 Jul 2010) pthreads
* Weave: 0.9.0
I have installed Docker 1.5 on both servers A and B, as well Weave version 0.9.0. One Docker container was started on each server on the same virtual network:
```
# On server A
./weave run 10.0.1.19/24 -ti --name haitao-19-w-1 \
          -v /home/htj:/home/htj wdc7:latest /bin/bash
          
# On server B
./weave run 10.0.1.20/24 -ti --name haitao-19-w-2 \
          -v /home/htj:/home/htj wdc7:latest /bin/bash
```
