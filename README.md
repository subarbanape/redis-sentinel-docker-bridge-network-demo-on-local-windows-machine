# Steps to get redis sentinel master slave work with bridge network based setup inside the docker

I have tried this on my local windows 10 machine with the docker desktop installed.

My end goal is to have the redis network setup in my company's local intranet and distrubute the redis instances and sentinels in different containers separated physically in different servers. But, prior to this, I wanted to give it a try on my local machine and familiarize all the naunces and surprises. After spending quite a bit of hours, following things work. This may not be a perfect setup for all the scenarios but its a good head-start. 

1. Everything is running on the same docker desktop machine.
2. Slave and Master able to successfully sync the data. I got 1 master and 1 slave.
3. Sentinels successfully able to elect the readers among them. I got 3 sentinels.
4. Sentinels successfully able to select the master when the current master is down and able to rediscover the former master when it is discovered.
5. New master successfully able to sync data when the former master (now slave) is discovered.

I used bridge network to host all the redis infrastructure. The config files now got hard coded ip address of my physcial host machine (IPv4 address) for the redis instances to be reachable for sentinels. I had issues using direct names and also localhost, 127.0.0.0, 0.0.0.0 didnt work. So, finally I found out using the machine ip address will do the trick. So, please replace those IP Addresses with your local machine's IP before testing.
