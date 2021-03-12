# Steps to get redis sentinel master slave work with bridge network based setup inside the docker

I have tried this on my local windows 10 machine with the docker desktop installed.

My end goal is to have the redis network setup in my company's local intranet. But, prior to this, I wanted to give it a try on my local machine and familiarize all the naunces and surprises. After spending quite a bit of hours, following things work. This may not be a perfect setup for all the scenarios but its a good head-start. 

1. Slave and Master able to successfully sync the data. I got 1 master and 1 slave.
2. Sentinels successfully able to elect the readers among them. I got 3 sentinels.
3. 

just show the configs to get the redis master-slave and sentinel work with bridge based docker network.
