# k8s-workshop-notes

Reset Workspace
---------------
Delete sample code

Delete Kubernetes resources
```
kubectl delete services k8sdemo
kubectl delete deployment k8sdemo
```

PKS Topics
----------

```
pks
pks login -a api.pks.jg-aws.com -u bryan -k
plans
pks clusters

#pks create-cluster bg-demo-cluster --external-hostname bryan.pks.jg-aws.com --plan small
```

Open Dashboard to show cluster



```
pks get-credentials bg-demo-cluster

kubectl get nodes 
```


Demo App
--------


https://start.spring.io/

Switcn to Gradle Project
Group: com.springdev.k8s
Artifact: k8sdemo
Add Web, Actuator dependencies

Generate Project

In terminal go to workspace
```
cp ~/Downloads/k8sdemo.zip .
unzip k8sdemo.zip
cd k8sdemo
```

Open the k8sdemo folder in VS Code

Add a new REST controller

```
@RestController
	class HelloController {
		@GetMapping("/hello")
		String hello() {
			return "Hello from K8S!";
		}
	}
```
Let IDE help add required imports

Open application.properties file and add:

Explain how this is just for demonstration, and never to do this. Disables actuator endpoint security, and exposes all endpoints.

```
management.security.enabled=false
management.endpoints.web.exposure.include=*
endpoints.env.enabled=true

```

Add a Dockerfile

```FROM openjdk:8-alpine
VOLUME /tmp
ADD ./build/libs/k8sdemo-0.0.1-SNAPSHOT.jar /k8sdemo.jar
RUN sh -c 'touch /k8sdemo.jar'
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/k8sdemo.jar"]
```

Build and package:

```
./gradlew build
docker build -t bgregorius77/k8sdemo:0.0.1 .
docker push bgregorius77/k8sdemo
```

Deploy
```
kubectl run k8sdemo --image bgregorius77/k8sdemo:0.0.1 --port=8080
kubectl expose deployment k8sdemo --type=LoadBalancer
```


Click through Dashboard to external url then add /hello

Show some of the Actuator endpoints:
/actuator/health
/actuator/metrics
/actuattor/env

