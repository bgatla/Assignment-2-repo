FROM tomcat:latest
LABEL maintainer="Balaji Gatla"
ADD ./target/newProj-0.0.1-SNAPSHOT.war /usr/local/tomcat/webapps/
EXPOSE 8080
CMD ["catalina.sh", "run"]