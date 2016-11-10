# How To Install and Configure golang on Ubuntu 16.04 Server

1. Navigate to $HOME directory, and download latest stable golang install package (current version is go.1.7):

   ```
   $ cd ~
   $ curl -O https://storage.googleapis.com/golang/go1.7.linux-amd64.tar.gz
   ```
   
2. Verify the tarball with sha256sum: 

   ```
   $ sha256sum go1.7.liunx-amd64.tar.gz
   ```
   sample output 
   
   ```
   $ sha256sum go1.7.linux-amd64.tar.gz
     702ad90f705365227e902b42d91dd1a40e48ca7f67a2fd052aaa4295cd95 go.1.7.linux-amd64.tar.gz
   ```
 
3. Use ```tar``` to extract the tarball.  ```x``` flag tells ```tar``` to extract, ```v``` enables verbose output, and ```f``` specifies a filename:

   ```
   $ tar xvf go1.7.linux-amd64.tar.gz
   ```
   
4. There should have a ```go``` directory in the home directory.  Recursively change ```go```'s owner and group to ***root***, and move it to ```/usr/local```:
   
   ```
   $ sudo chown -R root:root ./go
   $ sudo mv go /usr/go
   ```
  
5. Set go path

   ```
   $ sudo nano ~/.profile
   ```
   at the end of the file add the following lines:
   
   ```
   export GOPATH=$HOME/work
   export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
   ```
   Save and close the file, and refresh the file by running:
   
   ```
   $ source ~/.profile
   ```

6. Verify the installation

   the following steps will verify correct installation of golang 
   
   ```
   $ mkdir $HOME/work
   $ mkdir -p work/src/github.com/<userid>/hello
   $ nano ~/work/src/github.com/<userid>/hello/hello.go
   ```
   
   create a simple "hello" function
   
   ```
   package main 
   
   import "fmt"
   
   func main () {
   
      fmt.Printf ("hello, world/n")
   }
   ```
   
   compile the newly created "hello.go" with the following command: 
   
   ```
   $ go install github.com/<userid>/hello
   ```
   
   after correctly compile the file, run it with the following command: 
   
   ```
   $ hello
   ```
   
   if golang has been correctly installed, output should be: 
   
   ```
   hello, world
   ```
