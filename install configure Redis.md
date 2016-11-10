# How to install configure Redis on Ubuntu Server

1. First download and install ```build-essential``` meta-package from Ubuntu respostories, and also ```tcl``` package which is used to test binaries

  ```
  $ sudo apt-get update
  $ sudo apt-get install build-essentail tcl
  ```
  
2. Navigate to ```/tmp``` directory where we will build Redis

  ```
  $ cd /tmp
  ```
  
3. Download the latest stable Redis version:

  ```
  $ curl -O http://download.redis.io/redis-stable.tar.gz
  ```
  
4. Extract the tarball 

  ```
  $ tar xzvf redis-stable.tar.gz
  ```

5. Navigate to extracted directory

  ```
  $ cd redis-stable
  ```
  
6. Compile Redis binaries

  ``` 
  $ make
  ```

7. Run the test suite to verify everthing was built correctly:

  ```
  $ make test
  ```
  
8. once verified the build is correct, install the binaries onto the system:

  ```
  $ sudo make install
  ```
  
9. after successful install, create a configuration directory, use conventional ```/etc/redis``` directory

  ```
  $ sudo mkdir /etc/redis
  ```
  
10. copy the sample Redis configuration file from Redis source archive:

  ```
  $ sudo cp /tmp/redis-stable/redis.conf /etc/redis
  ```
  
11. Adjust the configuration file:

  ``` 
  $ sudo nano /etc/redis/redis.conf
  ```
  
  find ```supervised``` directive, default is set to ```no```, change it to ```systemd```:
  
  
  ```
                         /etc/redis/redis.conf
                           
  ...
  
  # If you run Redis from upstart or systemd, Redis can interact with your
  # supervision tree. Options:  
  #   supervised no      - no supervision interaction 
  #   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
  #   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
  #   supervised auto    - detect upstart or systemd method based on
  #   UPSTART_JOB or NOTIFY_SOCKET environment variables
  #   Note: these supervision methods only signal "process is ready."
  #   They do not enable continuous liveness pings back to your supervisor.
  supervised systemd
  
  ...
  ```
  
  find ```dir``` directroy, this specifies the directory where Redis will use to dump persistent data. use ```/var/lib/redis``` directory, which we will create later. 
  
    ```
                      /etc/redis/redis.conf
                      
  ...

  # The working directory.
  #
  # The DB will be written inside this directory, with the filename specified
  # above using the 'dbfilename' configuration directive.
  #
  # The Append Only File will also be created inside this directory.
  #
  # Note that you must specify a directory here, not a file name.
  dir /var/lib/redis
  
  ...
  ```
  
12. create a systemd unit file , create and open the ```/etc/systemd/system/redis.service``` file:

  ```
  $ sudo nano /etc/systemd/system/redis.service
  ```
  
  edit the file: 
  
  ```
                    /etc/systemd/system/redis.service
  
  [Unit]
  Description=Redis In-Memory Data Store
  After=network.target
  
  [Service]
  User=redis
  Group=redis
  ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
  ExecStop=/usr/local/bin/redis-cli shutdown
  Restart=always
  
  [Install]
  WantedBy=multi-user.target
  ```
  Save and close the file
  
13. Create user, group and directory referenced previously:

  ```
  $ sudo adduser --system --group --no-create-home redis
  $ sudo mkdir /var/lib/redis
  ```
  
  assign ```redis``` user and group ownership to the directory, and change permission so regular users cannot access this directory
  
  ```
  $ sudo chown redis:redis /var/lib/redis
  $ sudo chmod 770 /var/lib/redis
  ```
  
14.  start up the systemd service, and check service status is running with no error:

  ```
  $ sudo systemctl start redis
  $ sudo systemctl status redis
  ```
  
  should have the output look like the following:
  
  ```
  ● redis.service - Redis Server
   Loaded: loaded (/etc/systemd/system/redis.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2016-05-11 14:38:08 EDT; 1min 43s ago
  Process: 3115 ExecStop=/usr/local/bin/redis-cli shutdown (code=exited, status=0/SUCCESS)
 Main PID: 3124 (redis-server)
    Tasks: 3 (limit: 512)
   Memory: 864.0K
      CPU: 179ms
   CGroup: /system.slice/redis.service
           └─3124 /usr/local/bin/redis-server 127.0.0.1:6379
  ```

15. Test Redis instance functionality

  ```
  $ redis-cli
  127.0.0.1:6379> ping
  PONG
  ```
  
16. Verify able to set keys:

  ```
  127.0.0.1:6379> set test "it is working!"
  OK
  127.0.0.1:6379> get test
  "it is working!"
  127.0.0.1:6379> exit
  ```
  
  
  
  

  
