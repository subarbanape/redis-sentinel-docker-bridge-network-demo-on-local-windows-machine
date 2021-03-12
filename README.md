# Steps to get redis sentinel master slave work with bridge network based setup inside the docker

I have tried this on my local windows 10 machine with the docker desktop installed.

My end goal is to have the redis network setup in my company's local intranet and distrubute the redis instances and sentinels in different containers separated physically in different servers. But, prior to this, I wanted to give it a try on my local machine and familiarize all the naunces and surprises. After spending quite a bit of hours, following things work. This may not be a perfect setup for all the scenarios but its a good head-start. 

1. Everything is running on the same docker desktop machine.
2. Slave and Master able to successfully sync the data. I got 1 master and 1 slave.
3. Sentinels successfully able to elect the readers among them. I got 3 sentinels.
4. Sentinels successfully able to select the master when the current master is down and able to rediscover the former master when it is discovered.
5. New master successfully able to sync data when the former master (now slave) is discovered.

I used bridge network to host all the redis infrastructure. The config files now got hard coded ip address of my physcial host machine (IPv4 address) for the redis instances to be reachable for sentinels. I had issues using direct names and also localhost, 127.0.0.0, 0.0.0.0 didnt work. So, finally I found out using the machine ip address will do the trick. So, please replace those IP Addresses with your local machine's IP before testing.

Steps to test:

1. Open command prompt and cd to the redis folder.
2. Issue the command 'docker-compose up' to setup.
3. Observe that everything successfully created in your docker desktop window.
4. Now, open the CLI of master. Login to the redis container 'redis-cli -h 192.168.1.67 -p 6379' (replace the ip with your machine ip). Then, issue the command 'redis-cli info | grep ^role'. It should output master.
5. Repeat the above step for slave container. The output should say slave.
6. Now, open the CLI of one of the sentinels. And issue the command 'redis-cli -p 26379'. Then run all these commands and verify the redis instances are properly identified as master and slave. 
  (a) 'sentinel master redismaster' - should display master redis instance information. Observe that the flag should display master in the output. The port should be 6379.
  (b) 'SENTINEL replicas redismaster' - should display slave redis instance. Observe that the port should be 6380. The flag should display slave.
  (c) 'SENTINEL sentinels redismaster' - should display all the sentinels available.
  (d) 'SENTINEL get-master-addr-by-name redismaster' - should display the ip and the port of the current redis master. In this case, it will display the port as 6379.
  (e) 'info' - display good amount of information and the last line should have master ip and port, total master, slaves, sentinels.
7. Now, CLI on the redis master and issue 'redis-cli -h 192.168.1.67 -p 6379'. Then set the test key 'set testkey 123' and then issue 'get'. It should display the value 123.
8. Similary, repeat the above step on redis slave by issuing 'redis-cli -h 192.168.1.67 -p 6380'. It should also display the value 123 for the 'test' key. This is to verify if the master and slave are syncing each other.
9. Now, bring the redis master (6379) down by issuing the command 'redis-cli -h 192.168.1.67 -p 6379 DEBUG sleep 60' to make the master sleep temporarily for 60 seconds.
10. Now, observe the sentinels and verify they elect the current slave (6380) as master. 
11. Again issue commands on sentinels as above to see the current master.
12. Bring the old master up (or) wait for it to wake up. Now, this is a new slave. Now, both redis instances shoud in sync with the data. 
