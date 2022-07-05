## Summary
This repo has been created in order to showcase deployment of a sample java app in different platform such as vagrant, docker, kubernetes.
Also in this repo you can make use of ansible playbooks to install and configure required pakcages such as Java, Docker, Tomcat and for deployment. Please find the steps below.
## Kubernetes deploy in minikube
1. Create cluster by using link -: https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-interactive/
2. Clone the repo.
3. enable ingress in minikube by running below command.
```
     minikube addons enable ingress
```
4. Deploy manifest by running below command.
```
     kubectl create -f kube-compose.yml
```
5. Test the app by running below command.
```
     kubectl get deploy
     kubectl get po 
     kubectl get ing
     curl -v <ingress-address>/sample-war/
```
6. use below command to check the logs.
```
     kubectl logs <sample-app-pod> 
```
## Docker container 
1. Folllow below commands to deploy sample app in a docker container. Also you need to install docker engine in your windows or linux box.
```
     sudo docker build -t sample-app .
     sudo docker run -d -p 8080:8080 sample-app
```
2. Test the app by running below command in linux.  
```
     curl -v localhost:8080/sample-war/
```
3. For windows you can test app in the browser.
```
    http://localhost:8080/sample-war/
```
4. You can also push the same image to your docker hub repo by running below commands.
```
     docker login
     docker tag sample-app <docker-hub-id/sample-app>
     docker push <docker-hub-id/sample-app>
```

## Vagrant deploy sample app with ansible
1. Follow below steps to deploy sample app in a vagrant image. Also you need to install vagrant in your windows or linux box. you need to make sure that you have ansible installed in your vagrant box. Also if you are using vagrant in linux and you have ansible installed in the linux vm then vagrant will be able to provision ansible task automatically. 
```
     vagrant up
     vagrant provision # run this command if you have vagrant and ansible installed in your linux VM.
     vagrant ssh
```
2. Clone the repo in the vagrant machine and run below commands.
```
     cd dell-assignment
     ansible-playbook -i hosts playbook.yml
```
3. Test the app by running below command in linux.  
```
     curl -v localhost:8080/sample-war/
```
4. for windows you can test app in the browser.
```
    http://localhost:8081/sample-war/
```
# configmap
Configmap was required to print the logs in STDOUT and STDERR instead creating a log file for the application logs. This has been skipped since I don't have springboot api to deploy and therefore I no need to create configmap in kubernets manifest. 

However I have kept the logback.xml file which can be used to print the logs in stdout and stderr in spring boot api and can be mapped to the app pod by having configmap volume in the kubernetes deployment manifest. 

Refer to below steps for configmap creation:
1. Run command to create a configmap.
```
     kubectl create cm sample-app-config --from-file=logback.xml
     kubectl get cm
```
2. Mount the voulume to the pod. 
```
   spec:
      volumeMounts:
      - name: sample-app-conf-vol
        mountPath: /usr/local/tomcat/config/
    volumes:
    - name: sample-app-conf-vol
      configMap:
        name: sample-app-config
```
## Thanks

