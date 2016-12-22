Basic [Docker](https://www.docker.com/) image to run [Tomcat](https://tomcat.apache.org/) and [Java](https://www.java.com/) applications, based on [Debian](http://www.debian.org/) Jessie. This image is patched with Java Cryptographic Extension (JCE) Unlimited Strength Jurisdiction Policy Files.


### Usage

Some indications:

* Tomcat installation directory is `/opt/tomcat` (`$TOMCAT_HOME`/`$CATALINA_HOME`). Executable scripts are found in directory `$TOMCAT_HOME/bin` and the application base (*appBase*) directory is `$TOMCAT_HOME/webapps`.
* The path of file `catalina.out` is managed by the variable `$CATALINA_OUT`, and its value by default is `/dev/null` (disabled).
* Apache logs are written into directory `/logs/`.

Use the image directly, and copy the `.war` files directly into the *appBase* directory. For example:

  ```
  docker run -it --rm docker-debian-oracle-java8-tomcat8 /opt/tomcat/bin/catalina.sh run
  docker cp ./sample.war tomcat-ci:/opt/tomcat/webapps/sample.war
  ```
  
  
## Ports
Two ports are exposed:

 - 8080: default Tomcat port.
  
 - 8009: default Tomcat debug port.

Remember to map the ports to the docker host on run.

## Volumes
Exports a volume on `/opt/tomcat/webapps`.
You can mount the volume on run to a local directory containing your war file or exploded war directory.
If you need the management app, remember to have a copy on your hosts volume mount point.

# How to run the container
## Using docker
You need docker v1.3+ installed. To get the container up and running, run:
 
```
sudo docker run -d -p 8080:8080 -p 8009:8009 -v /opt/tomcat/webapps:/opt/tomcat/webapps dordoka/tomcat
```
Remember to change `/opt/tomcat/webapps` to the directory where your app is stored.

## Using docker compose
If you have `docker-compose` installed, you can just launch:

```
sudo docker-compose up
```

## A warning regarding admin user for tomcat management console
Please note that the image contains a `tomcat-users.xml` file, including an `admin` user (password `admin`). For the time being, should you wish to change that, fork this repo and modify the xml file accordingly.
