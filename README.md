# Time synchronization between two systems. 
Time Synchronization between Two Systems refers to the process of ensuring that two computer systems have the same accurate time, helping them work together efficiently and enabling tasks like data consistency and security measures.
You need to install **chrony** if not installed, This operation uses 284 kB of disk space.
```sh
sudo apt install chrony
```
### What makes Chrony a better choice than Ntpdate?
The `ntpdate` command does not provide the same detailed tracking and synchronization status information as the `chronyc tracking` command in Chrony. `ntpdate` is a simple tool for performing one-time time synchronization, and it doesn't offer real-time tracking and monitoring features.

 NTP has been around since the mid-1980s and has a long history, while Chrony is a more recent addition to the world of time synchronization, introduced in the early 2000s. Both protocols continue to be used, with Chrony being favored in many modern Linux distributions for its improved accuracy and network handling capabilities.
## Assumptions:
- System 1 (192.168.1.2) serves as the server.
- System 2 (192.168.1.3) acts as the client.

## Method 1: Server-Client Mode for Regular Time Synchronization
#### Server Configuration (System 1):
```sh
server 127.127.1.1 prefer
local stratum 10
allow 192.168.1.3
cmdallow 192.168.1.3
```

#### Client Configuration (System 2):
```sh
server 192.168.1.2 iburst minpoll 1 maxpoll 1
allow 192.168.1.2
cmdallow 192.168.1.2
```

###### Mode 1 - Server will use only local time and will not sync online time.
In this mode we just disabled the pool for online servers on System 1.

###### Mode 2 - Server will will local time and also sync time with online servers.
In this mode we just enabled the pool for online servers on System 1.
> After seting up server and client configuration file make sure to restart chrony service, use below command:
```sh
sudo service chrony restart
```

## Method 2: One-Time Synchronization
Client request for time synchronization using command line.

#### Server Configuration (System 1):
```sh
cmdallow 192.168.1.3
```
> After seting up server configuration file make sure to restart chrony service, use below command:
```sh
sudo service chrony restart
```
#### Client Configuration (System 2):
- Client does not require to change configuration, it uses default configuration.
- Run below command on terminal to check the <Leap status>- it should be normal
    ```sh
    watch -n1 chronyc tracking
    ```
- Use command line to sync time with system 1
    ```sh
    sudo chronyc makestep
    ```

## License
**Free Software, Hell Yeah!**

## Authors
- [Rahul Gupta](https://github.com/rahulelex)
