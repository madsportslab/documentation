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
  . . .

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
  
