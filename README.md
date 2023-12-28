# <p style="text-align: center;">Apache Tomcat with Podman</p>


### 1. Task Requirement

Set up Apache Tomcat on Podman, which is an alternative to Docker for running containers.

2. **Environment Details**
- **Operating System:** Ubuntu 20.04

**System Configuration:**
- **CPU:** Intel Core i5-8350U CPU
- **RAM:** 8GB (4GB x 2 SODIMM DDR4)
- **Storage:** 256GB
- Note: My system is equipped with an Intel Core i5-8350U CPU, 8GB
 RAM (4GB x 2 DDR4 SODIMM), and 512GB storage. However, this 
configuration may not be necessary for everyone.


3. **List of Tools and Technologies**
**- Apache Tomcat :-**
Apache Tomcat is an open-source web server and servlet container developed by the Apache Software Foundation. It serves as a robust platform for running Java-based web applications, particularly those that use Java Servlet, JavaServer Pages (JSP), WebSocket, and associated Java technologies.

**- Podman :-**
Podman is a Linux tool for creating and managing containers. It doesn't need a background daemon, enhancing security, and works well with system standards like OCI. It supports rootless containers, integrates with systemd, and handles container tasks like images, volumes, and networks efficiently.


4. **Command for the setup or configuration**

Step 1 : **Update system repositories.**

```
sudo apt update
```

- sudo: This part of the command is used to execute the following command with administrative or superuser privileges.

- apt: Refers to the APT package manager, which is commonly used on Debian-based Linux distributions like Ubuntu.

- update: This is the action you want APT to perform. When you run "sudo apt update," it instructs APT to update the package lists and information about available software packages from the configured repositories.


```
sudo apt upgrade
```

- sudo: This part of the command is used to execute the following command with administrative or superuser privileges.

- apt: Refers to the APT package manager, which is commonly used on Debian-based Linux distributions like Ubuntu.
- Upgrade : Upgrade, means to replace an older version of software with a newer one, bringing improvements, fixes, or new features to enhance its performance or capabilities.
- 
Step 5 : **Install podman.**

```
sudo apt install podman  
```

Problem : If you encountered an error while attempting to install Podman.

For solving this problem run below commands.

```
sudo sh -c "echo 'deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_$(lsb_release -rs)/ /' > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list"
```

```
wget https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_$(lsb_release -rs)/Release.key

```

```
sudo apt-key add - < Release.key

```

```
sudo apt install -y podman

```

```
podman --version

```

"Note : Adding a repository is required for Ubuntu 20 version and is not needed for the latest version of Ubuntu."

Step 6 : **Pull Tomcat Docker Image**

You can pull the official Tomcat Docker image from Docker Hub. Open your terminal and run:

```
podman pull tomcat:latest
```

Output :
```
jatin@jatin1:~$ podman pull tomcat
✔ docker.io/library/tomcat:latest
Trying to pull docker.io/library/tomcat:latest...
Getting image source signatures
Copying blob 74594af5feb5 done  
Copying blob 66bf1e8cc497 done  
Copying blob 43f89b94cd7d done  
Copying blob 190e928f9c42 done  
Copying blob a4452d37e1e4 done  
Copying blob 39b9b405c53f done  
Copying blob 4cdcdf959377 done  
Copying blob d9c9ec61c8dd done  
Copying config 3db0f5668a done  
Writing manifest to image destination
Storing signatures
3db0f5668a77664c221b187b735d86ed63b1d3b98a15d4e80676b13b9b155fdc
```
- The command "podman pull tomcat" is used to download (or pull) the Docker image named "tomcat" from a container registry. This fetches the specified image, in this case, "tomcat," which typically contains the Tomcat web server, and makes it available locally for use in creating and running containers with Podman.

Step 7 :  **Create a Podman Container**

```
podman run -d -p 8081:8080 --name my-tomcat docker.io/library/tomcat
```

Output :
```
jatin@jatin1:~$ podman run -d -p 8081:8080 --name my-tomcat docker.io/library/tomcat
d1cb2f106e2ce508755a24126b10b904325054faed69418071a8fc70c73d09e9

```

- -d: Runs the container in detached mode.
- -p 8081:8080: Maps port 8081 in the container to port 8080 on the host.
- --name my-tomcat: Names the container "my-tomcat."


##### Step 8 : Check container 

```
podman ps -a
```
Output :
```
jatin@jatin1:~$ podman ps 
CONTAINER ID  IMAGE                            COMMAND          CREATED             STATUS                 PORTS                   NAMES
d1cb2f106e2c  docker.io/library/tomcat:latest  catalina.sh run  About a minute ago  Up About a minute ago  0.0.0.0:8081->8080/tcp  my-tomcat
```

- List the currently running containers managed by the Podman tool.
- Provides information about container IDs, names, status, ports, and more.
- Using "-a" in commands means including or considering all items, not just the ones currently active or visible.

##### Step 9 : Enter Tomcat Container

```
podman exec -it my-tomcat /bin/bash
```
Output :

```
jatin@jatin1:~$ podman exec -it my-tomcat /bin/bash
root@d1cb2f106e2c:/usr/local/tomcat#
```



podman exec: This part of the command tells Podman to execute a command within an existing container.

- -it: "-it" is like choosing to have a chat with the container, letting you interact and communicate with it as if you're using a real-time conversation.

- my-tomcat: This is the name of the container you want to access. In this case, it's "my-tomcat."

- /bin/bash: This is the command that you want to run inside the container. It launches a Bash shell, providing you with an interactive terminal session within the container.

##### Step 10 : Changed The Current Directory

```
cd webapps.dist/

```
Output :
```
root@d1cb2f106e2c:/usr/local/tomcat# cd webapps.dist/
root@d1cb2f106e2c:/usr/local/tomcat/webapps.dist# 
```


- The command cd webapps.dist/ is used to change the current working directory to a directory called "webapps.dist."

##### Step 11 : Check List of Files 

```
ls
```
Output :
```
root@d1cb2f106e2c:/usr/local/tomcat/webapps.dist# ls
docs  examples  host-manager  manager  ROOT
root@d1cb2f106e2c:/usr/local/tomcat/webapps.dist#
```

##### Step 12 : Copy All The Contents

```
cp -R * ../webapps
```
Output :
```
root@d1cb2f106e2c:/usr/local/tomcat/webapps.dist# cp -R * ../webapps
root@d1cb2f106e2c:/usr/local/tomcat/webapps.dist# 
```
cp: This is the command for 'copy.'
-R: It stands for 'recursive,' which means it will copy directories and their contents recursively.
*: This is a wildcard character that represents all files and directories in the current directory.
../webapps: This is the destination directory where the files and directories will be copied. The '..' refers to the parent directory, and 'webapps' is the directory within the parent directory where the content will be copied.



##### Step 13 : Change Directory

```
cd ..
```
Output :
```
root@d1cb2f106e2c:/usr/local/tomcat/webapps.dist# cd ..
root@d1cb2f106e2c:/usr/local/tomcat# 
```



##### Step 14 : Changed The Current Directory

```
cd webapps/
```
- cd webapps: You changed the current directory to webapps again

Output :
```
root@d1cb2f106e2c:/usr/local/tomcat# cd webapps/
root@d1cb2f106e2c:/usr/local/tomcat/webapps#
```

##### Step 15 : Check List of Files 

```
ls
```

Output :

```
root@d1cb2f106e2c:/usr/local/tomcat/webapps# ls
docs  examples  host-manager  manager  ROOT
root@d1cb2f106e2c:/usr/local/tomcat/webapps#
```



- ls: Finally, you used the ls command to list the contents of the webapps directory, which should now include the directories docs, examples, host-manager, manager, and ROOT. This indicates that you've successfully copied the contents from webapps.dist to webapps

##### Step 16 : Access Deployed Web Application

- You can access your deployed web application by visiting http://localhost:8081/your-app in your web browser. Replace "your-app" with the name of your deployed application.

![Alt text](image/11.png)

