Spring application

1. Mark folder containing Java file as Sources Root.

2\. pom.xml file template for Spring Application





<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0"

         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>



    <parent>

        <groupId>org.springframework.boot</groupId>

        <artifactId>spring-boot-starter-parent</artifactId>

        <version>2.7.18</version>

    </parent>



    <groupId>org.example</groupId>

    <artifactId>TestTrascribeProject</artifactId>

    <version>1.0-SNAPSHOT</version>



    <properties>

        <maven.compiler.source>8</maven.compiler.source>

        <maven.compiler.target>8</maven.compiler.target>

        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>

    </properties>



    <dependencies>

        <dependency>

            <groupId>org.springframework.boot</groupId>

            <artifactId>spring-boot-starter-web</artifactId>

        </dependency>

    </dependencies>



</project>



3\. imports needed for spring application in main java file.



import org.springframework.boot.SpringApplication;



--> To run SpringApplication.run(com.example.springapp.Main.class, args); inside the main function.



import org.springframework.boot.autoconfigure.SpringBootApplication;



--> To write the annotation @SpringBootApplication



4\. A controller class sample as below



package org.example;



import org.springframework.web.bind.annotation.GetMapping;

import org.springframework.web.bind.annotation.RestController;



@RestController

public class HelloController {



    @GetMapping("/hello")

    public String hello() {

        return "Hello World";

    }

}



5\. com.example.springapp.Main class and controller needs to be inside a package.





***Git***



1. install git for windows



2\. initiate git, add files and make a commit.



git init

git add .

git commit -m "Initial Spring Boot app"



3\. Set user info on git



git config --global user.name "Your Name"

git config --global user.email "your\_github\_email@example.com"



4\. Connect to a GitHub repo (You should create a repo at GitHub first)



git remote add origin https://github.com/ibrahimmehkri/SpringApp.git

git branch -M main

git push -u origin main


5. Tools discussed in the meeting with Devex.

Here are **all tools mentioned**, categorized clearly:

---

## 🧱 Container & Orchestration

* **Docker**
* **Kubernetes**
* **Helm**
* **Rancher**
* **Argo CD**

---

## 🔁 CI/CD & Version Control

* **Jenkins**
* **Bitbucket**
* **Git (GitOps)**
* **ADR repo**
* **MCR (Microsoft Container Registry)**

---

## 📊 Monitoring & Logging

* **Grafana**
* **Prometheus**
* **InfluxDB**
* **Greylog**

---

## 🧩 Internal / Custom Tools

* **Utvecklingskatalogen**
* **IT-verktyg**
* **Modellkatalogen**
* **NIG Version Service**
* **CCD Demo (example app)**

---

## 🐳 Container Configuration Files

* **Dockerfile**
* **Dockerfile-test**
* **Helm values.yaml**
* **ConfigMaps**
* **Secrets**

---

## 🌐 Infrastructure Concepts (Platform Features)

* **Ingress**
* **Service (Kubernetes service)**
* **Namespace**
* **Network Policies**
* **Deployment**
* **Pods**

---

6. Step one is to create a jar file. Use Maven window, under lifecycle double click Package. You will see a jar file under project/target folder. 

7. Check if the jar is able to run 

java -jar .\target\YOUR-JAR-NAME.jar


8. Add this to pom.xml to create a Spring boot jar file rather than a plain jar file. 

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>

9. Create file called Dockerfile in project root. and use this. 

FROM eclipse-temurin:8-jre
WORKDIR /app
COPY target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]

**Desc: eclipse-temurin:<version> this is a base image from Docker hub. What does it contain? It contains Linux User space and JRE (Which contains JVM). What is a user space? 

The term user space (or userland) refers to all code that runs outside the operating system's kernel.[2] User space usually refers to the various programs and libraries that the operating system uses to interact with the kernel: software that performs input/output, manipulates file system objects, application software, etc.

WORKDIR /app tells docker to create a folder inside the container filesystem called app.

ENTRYPOINT this is a java binary that exists inside the container, docker gets the kernel to launch this java exec and run the app. So the container essentially becomes a java process with its isolate filesystem and network using the OS kernel. 

10. Install Docker desktop. And also WSL2. Because containers need Linux kernels. WSL2 is a lightweight Linux VM for windows. 

11. Build the image. 

docker build -t firstspringapp:1.0 .

Deployments Pods Services ConfigMaps Secrets??

12. Check if docker image created. 

docker images

13. Run docker container

docker run -p 8080:8080 firstspringapp:1.0

14. Install minikube and Kubernetes cli 

choco install minikube kubernetes-cli -y

15. Run Kubernetes cluster 

minikube start --driver=docker

check status with minikube status 


**************************************************************************

Some Kubernetes lingo: Kubernetes resources (Pods, deployments, configmap??, secret??, namespaces).  

Kubernetes cluster can be thought of as a database with the following tables. 

Table: Nodes
Table: Namespaces
Table: Deployments
Table: Pods
Table: Services
Table: ConfigMaps
Table: Secrets

Kubernetes also has controllers, that check the desired state in the database and implement changes in the actual state (processes running)

Control plane (brain) and worker nodes (muscle). Kubernetes also exposes API (endpoints) that u use to communicate with the control plane. One control plane per cluster. One API Server per cluster. 

16. Create a Deployment.yaml file in project root. 

apiVersion: apps/v1
kind: Deployment
metadata:
  name: firstspringapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: firstspringapp
  template:
    metadata:
      labels:
        app: firstspringapp
    spec:
      containers:
        - name: firstspringapp
          image: firstspringapp:1.0
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 8080

17. minikube image load firstspringapp:1.0

Do this to get the docker image into minikube

18. minikube image ls 

Here u can see the new image added.

19. Run this 

kubectl apply -f deployment.yaml

The yaml file is going to be evaluated by the API Server, apiVersion: apps/v1 is just a schema for a table, and if it is correct, it will be added to the etcd which is where Kubernetes keeps state, one etcd per cluster. Change in etcd will activate the Deployment controller and it will result in creating a pod for the application. 

kubectl get pods

****************************************************************

Application Repo
      │
      │ (push to master)
      ▼
Jenkins (CI)
  - Build image
  - Push image to registry
  - Update ADR repo (new image tag)
      │
      ▼
Application Deployment Repo (ADR)
      │
      │ (Argo watches this repo)
      ▼
Argo CD
  - Detects change
  - Applies manifests to cluster
      │
      ▼
Kubernetes
  - Deployment controller rolls out new pods

 
20. We need to access the api of the app. In order to do that we need a Service. Usually one service per deployment. 

http://firstspringapp-svc:80/hello

Any other pod in the same cluster can use this to communicate with our app thru its service. But in order for this app to be accessible from outside the cluster we need to use a tool like ingress. 



