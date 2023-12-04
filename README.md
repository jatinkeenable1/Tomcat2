# <p style="text-align: center;">TOMCAT SETUP IN PODMAN</p>


### 1. Task Requirement

Set up Apache Tomcat on Podman, which is an alternative to Docker for running containers.
### 2. Definition Tomcat
Apache Tomcat, often referred to as Tomcat, is an open-source application server developed by the Apache Software Foundation. It is a popular choice for running Java-based web applications. Tomcat serves as a Java Servlet and JavaServer Pages (JSP) container, providing a runtime environment for executing these Java technologies.

###3. Definition Podman 
Podman is an open-source container management tool used for running and managing containerized applications on Linux-based systems.

###4. Environment Details
- **Operating System:** Ubuntu 20.04

**System Configuration:**
- **CPU:** Intel Core i3-8350U CPU @ 1.70GHz x 8
- **RAM:** 8GB (4GB x 2 SODIMM DDR4)
- **Storage:** 256GB

###5. List of Tools and Technologies
- Tomcat
- Podman 

###6. Command for the setup or configuration

##### Step 1 : Update system repositories.

```
sudo apt update
```

- sudo: This part of the command is used to execute the following command with administrative or superuser privileges.

- apt: Refers to the APT package manager, which is commonly used on Debian-based Linux distributions like Ubuntu.

- update: This is the action you want APT to perform. When you run "sudo apt update," it instructs APT to update the package lists and information about available software packages from the configured repositories.

##### Step 2 : Install podman.

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
sudo apt update

```

```
sudo apt install -y podman

```

```
podman --version

```

"Note : Adding a repository is required for Ubuntu 20 version and is not needed for the latest version of Ubuntu."

##### Step 3 : Pull Tomcat Docker Image

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


##### Step 4 :  Create a Podman Container

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


##### Step 5 : Check container 

```
podman ps 
```
Output :
```
jatin@jatin1:~$ podman ps 
CONTAINER ID  IMAGE                            COMMAND          CREATED             STATUS                 PORTS                   NAMES
d1cb2f106e2c  docker.io/library/tomcat:latest  catalina.sh run  About a minute ago  Up About a minute ago  0.0.0.0:8081->8080/tcp  my-tomcat
```

- List the currently running containers managed by the Podman tool.
- Provides information about container IDs, names, status, ports, and more.

##### Step 6 : Enter Tomcat Container

```
podman exec -it my-tomcat /bin/bash
```
Output :

```
jatin@jatin1:~$ podman exec -it my-tomcat /bin/bash
root@d1cb2f106e2c:/usr/local/tomcat#
```



podman exec: This part of the command tells Podman to execute a command within an existing container.

- -it: These are options used to make the interaction with the container's shell interactive. "i" stands for interactive, and "t" allocates a pseudo-TTY for the shell.

- my-tomcat: This is the name of the container you want to access. In this case, it's "my-tomcat."

- /bin/bash: This is the command that you want to run inside the container. It launches a Bash shell, providing you with an interactive terminal session within the container.

##### Step 6 : Changed The Current Directory

```
cd webapps.dist/

```
Output :
```
root@d1cb2f106e2c:/usr/local/tomcat# cd webapps.dist/
root@d1cb2f106e2c:/usr/local/tomcat/webapps.dist# 
```


- The command cd webapps.dist/ is used to change the current working directory to a directory called "webapps.dist."

##### Step 7 : Check List of Files 

```
ls
```
Output :
```
root@d1cb2f106e2c:/usr/local/tomcat/webapps.dist# ls
docs  examples  host-manager  manager  ROOT
root@d1cb2f106e2c:/usr/local/tomcat/webapps.dist#
```

##### Step 8 : Copy All The Contents

```
cp -R * ../webapps
```
Output :
```
root@d1cb2f106e2c:/usr/local/tomcat/webapps.dist# cp -R * ../webapps
root@d1cb2f106e2c:/usr/local/tomcat/webapps.dist# 
```
- cp -R * ../webapps: Here, you've used the cp command to copy all the contents (including subdirectories and files) of the webapps.dist directory to the webapps directory located one level above. The -R flag indicates a recursive copy to ensure that all contents are copied.



##### Step 9 : Change Directory

```
cd ..
```
Output :
```
root@d1cb2f106e2c:/usr/local/tomcat/webapps.dist# cd ..
root@d1cb2f106e2c:/usr/local/tomcat# 
```



##### Step 10 : Changed The Current Directory

```
cd webapps/
```
- cd webapps: You changed the current directory to webapps again

Output :
```
root@d1cb2f106e2c:/usr/local/tomcat# cd webapps/
root@d1cb2f106e2c:/usr/local/tomcat/webapps#
```

##### Step 10 : Check List of Files 

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

##### Step 11 : Access Deployed Web Application

- You can access your deployed web application by visiting http://localhost:8081/your-app in your web browser. Replace "your-app" with the name of your deployed application.

![Alt text](image/11.png)

