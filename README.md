# cloudfoundry

Provides information on how to use the [java test application](https://github.com/cloudfoundry/java-test-applications) provided, build and deploy to IBM Cloud Private Cloud Foundry.

# pre requisite
- spring boot cli
- gradle
- JDK 7
- internet connectivity

# Clone the test application
```
git clone https://github.com/cloudfoundry/java-test-applications
cd java-test-applications
```

# ensuing your environment is valid
Keeps running the following command until you see **BUILD SUCCESSFULL** where it downloaded all dependencies files.
```
./gradlew check
```

## build all the application
```
./gradlew build
```

# Test application
to test the application, change to the folder application of choice.

Example : to test the web application
## append the following line into your your /etc/hosts file
```
192.168.64.162  web-application.apps.cf.sgcc.demo.lan
```

## build the sample application
```
cd web-application
```

### build the application if the build folder is not found
```
gradle build --stacktrace
```

## Deploy the application

```
cf api https://api.mgmt.cf.sgcc.demo.lan --skip-ssl-validation
cf login -u <user-name> -p  <user-password>
cf push
```

## validate application is running
```
cf apps
```

### Expected output
```
name                     requested state   instances   memory   disk   urls
web-application          started           1/1         1G       1G     web-application.apps.cf.sgcc.demo.lan
discovery-news-iccdemo   started           1/1         256M     1G     discovery-news-iccdemo.apps.cf.sgcc.demo.lan
```

## access the application
### All applications support the following REST operations:

| URI | Description
| --- | -----------
| `GET  /` | The health of the application
| `GET  /class-path` | The classpath of the application
| `GET  /datasource/check-access` | The ability of the application to access a RDBMS
| `GET  /datasource/url` | The URL of the application's `DataSource`
| `GET  /environment-variables` | The environment variables available to the application
| `GET  /input-arguments` | The list of JVM input arguments for the application
| `GET  /mongodb/check-access` | The ability of the application to access MongoDB
| `GET  /mongodb/url` | The URL of the application's MongoDB
| `POST /out-of-memory` | The URL to trigger an out of memory error
| `GET  /rabbit/check-access` | The ability of the application to access RabbitMQ
| `GET  /rabbit/url` | The URL of the application's RabbitMQ
| `GET  /redis/check-access` | The ability of the application to access Redis
| `GET  /redis/url` | The URL of the application's Redis
| `GET  /request-headers` | The http request headers of the current request
| `GET  /security-providers` | The system security providers available to the application
| `GET  /system-properties` | The system properties available to the application

### GET /
```
curl -i http://web-application.apps.cf.sgcc.demo.lan/
```
### Expected output
```
HTTP/1.1 200 OK
Content-Length: 2
Content-Type: text/plain;charset=ISO-8859-1
Date: Sun, 17 Dec 2017 15:02:37 GMT
X-Vcap-Request-Id: 4e77311b-1234-499a-7a18-78c3c3f98aeb
```

### GET  /class-path
```
curl -i http://web-application.apps.cf.sgcc.demo.lan/class-path
```
### Expected output
```
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Date: Sun, 17 Dec 2017 15:19:22 GMT
X-Vcap-Request-Id: d4a7e2b3-8f73-4ade-76bf-99147c7ace86
Content-Length: 119

["/home/vcap/app/.java-buildpack/tomcat/bin/bootstrap.jar","/home/vcap/app/.java-buildpack/tomcat/bin/tomcat-juli.jar"]
```
### GET  /environment-variables
```
curl -i http://web-application.apps.cf.sgcc.demo.lan/environment-variables
```
### Expected output
```
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Date: Sun, 17 Dec 2017 15:19:12 GMT
X-Vcap-Request-Id: c5a3a77e-50bc-4a59-4728-5779bce530c1
Transfer-Encoding: chunked

{"CF_INSTANCE_ADDR":"192.168.64.181:60082","CF_INSTANCE_GUID":"5943b266-bcd6-4b9b-6331-af995c1e86f5","CF_INSTANCE_INDEX":"0","CF_INSTANCE_IP":"192.168.64.181","CF_INSTANCE_PORT":"60082","CF_INSTANCE_PORTS":"[{\"external\":60082,\"internal\":8080},{\"external\":60083,\"internal\":2222}]","HOME":"/home/vcap/app","INSTANCE_GUID":"5943b266-bcd6-4b9b-6331-af995c1e86f5","INSTANCE_INDEX":"0","JAVA_HOME":"/home/vcap/app/.java-buildpack/open_jdk_jre","JAVA_OPTS":"-agentpath:/home/vcap/app/.java-buildpack/open_jdk_jre/bin/jvmkill-1.12.0_RELEASE=printHeapHistogram=1 -Djava.io.tmpdir=/home/vcap/tmp -Djava.ext.dirs=/home/vcap/app/.java-buildpack/container_security_provider:/home/vcap/app/.java-buildpack/open_jdk_jre/lib/ext -Djava.security.properties=/home/vcap/app/.java-buildpack/java_security/java.security  -Daccess.logging.enabled=false -Djava.endorsed.dirs=/home/vcap/app/.java-buildpack/tomcat/endorsed -Dhttp.port=8080 -XX:MaxDirectMemorySize=10M -XX:MaxMetaspaceSize=120280K -XX:ReservedCodeCacheSize=240M -Xss1M -Xmx416295K -Djdk.tls.ephemeralDHKeySize=2048 -Djava.protocol.handler.pkgs=org.apache.catalina.webresources","JDK_JAVA_OPTIONS":" --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED","LANG":"en_US.UTF-8","MEMORY_LIMIT":"1024m","PATH":"/usr/local/bin:/usr/bin:/bin","PORT":"8080","PWD":"/home/vcap/app","SHLVL":"0","TMPDIR":"/home/vcap/tmp","USER":"vcap","VCAP_APPLICATION":"{\"application_id\":\"d60cf18c-1ea7-4c2e-b9d0-21771207ddfb\",\"application_name\":\"web-application\",\"application_uris\":[\"web-application.apps.cf.sgcc.demo.lan\"],\"application_version\":\"5325b833-3efd-4b59-9eb8-f747adaf1cc1\",\"host\":\"0.0.0.0\",\"instance_id\":\"5943b266-bcd6-4b9b-6331-af995c1e86f5\",\"instance_index\":0,\"limits\":{\"disk\":1024,\"fds\":16384,\"mem\":1024},\"name\":\"web-application\",\"port\":8080,\"space_id\":\"a5962966-e85a-46f6-947b-9cdaca9c909c\",\"space_name\":\"myspace\",\"uris\":[\"web-application.apps.cf.sgcc.demo.lan\"],\"users\":null,\"version\":\"5325b833-3efd-4b59-9eb8-f747adaf1cc1\"}","VCAP_SERVICES":"{}","authorization_endpoint":"https://login.mgmt.cf.sgcc.demo.lan/UAALoginServerWAR","cloud_controller_url":"https://api.mgmt.cf.sgcc.demo.lan"}
```



# Reference
- [start Gradle from scrach](https://spring.io/guides/gs/gradle/#scratch)
