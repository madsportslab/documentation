# How To Install And Configure nginx on Ubuntu 16.04 

1.  Add Nginx repository, below command will add the Nginx 1.11.0 repository

  ```
  $ sudo add-get-repository ppa:chris-lea/nginx-devel
  ```
 
2.  If you have not updated your Ubuntu installation, update, otherwise, go to step 3

  ```
  $ sudo apt-get update
  ```
  
3. Install Nginx

  ```
  $ sudo apt-get install nginx
  ```
  
## Useful Nginx commands

To start Nginx service 

  ```
  $ service nginx start
  ```
  
To stop Nginx service

  ```
  $ service nginx stop
  ```
  
To check Nginx service status

  ```
  $ service nginx status
  ```
  
To restart Nginx service 

  ```
  $ service nginx restart
  ```
  
To check current Nginx version

  ```
  $ nginx -V
  ```
To uninstall / remove Nginx installation

  ```
  $ sudo apt-get remove nginx
  ```
