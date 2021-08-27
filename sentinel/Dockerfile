FROM java:8
ADD sentinel-dashboard-1.8.2.jar app.jar
EXPOSE 8888
EXPOSE 8712
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-Dserver.port=8888","-Dcsp.sentinel.dashboard.server=localhost:8888","-Dproject.name=sentinel-dashboard","-jar","app.jar"]
