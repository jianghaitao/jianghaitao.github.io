# Weave / Docker Network Performance Tests
I was looking for some network performance numbers for [Weave](https://github.com/zettio/weave), but was not very successful. So, I decided to test it myself. 
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
Note that I mount the host directory so containers can share the resources like iperf with their Docker hosts.

## Network Latency
Network latency was tested using the flooded ping for more reliable results, e.g. ping -f -c 16 10.0.1.19 from 10.0.1.20.

     | Host - Host | Host - Container | Container - Container
---- | ----------- | ---------------- | ---------------------
min	| 0.088ms	| 0.094ms	| 0.475ms
avg |	0.105ms		| 0.128ms		| 0.684ms
max |	0.247ms		| 0.327ms		| 0.968ms
mdev |	0.038ms	| 0.0608ms		| 0.1371ms 

## TCP Throughput
TCP throughput was measured by the standard iperf tools. for exmaple
```
# From container 10.0.1.20
./iperf -c 10.0.1.19 -p 19765 -t 15 -i 1 -f m -r
```
     | Host - Host | Host - Container | Container - Container |
 ---- | --------- | ------- | -------- | -------------
 Mbps | 641       | 641  | 656 |
 
## Summary
My initial tests showed that Weave incurred some **30%** degradation on the TCP throughput and 6.51X increase in network latency when it comes to the container to container communication. Honestly, I was a bit surprised. Acccording to [Introducing flannel: An etcd backed overlay network for containers](https://coreos.com/blog/introducing-rudder/), "flannel introduces non-trivial latency penalty, it has almost no affect on the bandwidth.". I didn't expect Weave to be this much different.

I am curious to know the reasons behind the [Weave](https://github.com/zettio/weave) TCP bandwidth degradation and if there is way to improve. Any comments and suggestions are [welcomed](mailto:jianghaitao@gmail.com).
