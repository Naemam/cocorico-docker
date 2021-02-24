> ### Colors legend:
> <pre><mark>cocorico</mark> - replace the highlighted values after copy-pasting</pre>
> ```diff
> - ASSISTANCE NEEDED (thank you in advance, COCOLABS and www cocorico dev gurus!)
> ```
> ```diff
> + HELPFUL ADDITIONS?!
> ```

# How To Install and Setup Cocorico on Ubuntu 20.04 from the official [Cocorico docker image](https://hub.docker.com/r/cocolabs/cocorico/)

## Prerequisites
Ubuntu 20.04 with the following:

* [Docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)

* ```diff
  + Apache2

* ```diff 
  + PORTS: Cocorico needs the following ports available to run:
  
  :8080 (for web interface)
  :3306 (for mariadb/mysql)
  :9001 (for Supervisor Status)
  :27017 (for mongodb)
  ```

## Step 1 — Installing Cocorico
First, verify your docker installation:

```
docker -v
```

<sup>By default, the docker command can only be run by the root user or by a user in the docker group, which is automatically created during Docker’s installation process. The rest of this how-to assumes you are running the docker command as a user in the docker group. If you choose not to, please prepend the commands with sudo</sup>

```
docker pull cocolabs/cocorico
```

To verify the image has been downloaded to your machine, type:

```
docker images
```

In the directory where Cocorico will be installed (e.g. <code>/home/<mark>_username</mark>/<mark>_projects</mark></code>), create the directory for <code><mark>your_cocorico</mark></code> and change into it:

<pre>
mkdir <mark>your_cocorico</mark>
cd <mark>your_cocorico</mark>
</pre>

Finally, install Cocorico:

<pre>
docker run --name <mark>your_cocorico</mark> -ti -p 8080:8080 -p 3306:3306 -p 9001:9001 -p 27017:27017  -v `pwd`:/cocorico -v `pwd`/tmp/mysql:/var/lib/mysql -v `pwd`/tmp/mongo:/data/db -e HOST_UID=$UID cocolabs/cocorico
</pre>

```diff
- PORTS: Can the 4 port numbers be changed if necessary?
- Running the above with -p 80:80 as instructed in cocorico-docker documentation failed:

https://github.com/Cocolabs-SAS/cocorico-docker 

- Changing it to -p 8080:8080 as suggested here succeeded:

https://github.com/Cocolabs-SAS/cocorico-docker/issues/2
```

A list of symfony operations should start. If it finishes with `cocorio is alive` - congratulations!

The following docker commands come very handy if running into problems:

```
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
docker system prune
```

Now let's set it up!

## Step 2 — Setting up Cocorico (dev)
First, let's take care base variables and environment:
```diff
- Edit /etc/apache2/sites-available/000-default.conf ?
- Add something like this (what are the correct ServerName ServerAlias and DocumentRoot)?:
```
```
<VirtualHost *:8080>
	ServerName your_cocorico
    ServerAlias your_cocorico

    #For multiple images uploads
    LimitRequestBody 240000000

    DocumentRoot /home/_username/_projects/your_cocorico/web
    <Directory /home/_username/_projects/your_cocorico/web>
    	#For performance and security reasons we should not use htaccess in prod
        AllowOverride Indexes FileInfo AuthConfig
        Order Allow,Deny
        Allow from all
    </Directory>
</VirtualHost>
```

```diff
- What to do after connecting to the container?

- What are the urls to access admin and landing page?

- What are the admin credentials for first login?
```
The outputs currently are, respectively:
```
http://localhost/

>>"Apache2 Ubuntu Default Page"
```
```
http://localhost:8080/

>>"This site can’t be reached
ERR_SOCKET_NOT_CONNECTED"
```
```
http://localhost:3306/

>>"5.5.5-10.2.22-MariaDB-log  mysql_native_password Got packets out of order"
```
```
http://localhost:9001/

>>"Supervisor status"
```
```
http://localhost:27017/

>>"It looks like you are trying to access MongoDB over HTTP on the native driver port."
```

## Step 3 — Deploying Cocorico (prod)