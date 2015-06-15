# Kubernetes on docker (single node)

Based on  [Getting started][1] & [Kubernetes with docker][2]

1. Prerequisites
 * Docker
 * Docker-compose
 * [Kubectl binary][3] (see [Releases][4])
  ```
   $ wget http://storage.googleapis.com/kubernetes-release/release/v0.18.2/bin/linux/amd64/kubectl -O /usr/local/bin/kubectl
  $ chmod +x /usr/local/bin/kubectl
  $ kubectl version

  ```
1. Start
 * ```$ ./start.sh```
2. Test installation
 * 
 ```
     $ kubectl get nodes
     NAME        LABELS    STATUS
     127.0.0.1   <none>    Ready
 ```
 * 
 ```
     $ kubectl get pods
     POD                    IP           CONTAINER(S)         IMAGE(S)                                     HOST                  LABELS         STATUS    CREATED      MESSAGE
     k8s-master-127.0.0.1                                                                                  127.0.0.1/127.0.0.1   <none>         Running   16 minutes
                                         scheduler            gcr.io/google_containers/hyperkube:v0.17.0                                        Running   16 minutes
                                         apiserver            gcr.io/google_containers/hyperkube:v0.17.0                                        Running   16 minutes
                                         controller-manager   gcr.io/google_containers/hyperkube:v0.17.0                                        Running   16 minutes
 ```
3. Run tomcat
 * Create single node cluster
 * Create service
 ```
    $ kubectl create -f tomcat7-service.yaml
 ```
 * Check service created
 ```
    $ kubectl get services tomcat7
      NAME      LABELS         SELECTOR       IP(S)          PORT(S)
      tomcat7   name=tomcat7   name=tomcat7   10.0.0.191     8080/TCP
                                        192.168.1.66   
 ```
 * Create replica controller
 ```
    $ kubectl create -f tomcat7-controller.json
 ```
 * Check replica controller created
 ```
   $ kubectl get rc tomcat7
     CONTROLLER   CONTAINER(S)   IMAGE(S)                    SELECTOR       REPLICAS
     tomcat7      tomcat7        healthlink.docker/tomcat7   name=tomcat7   1
 ```
 * Check service and controller created
 ```
    $ kubectl get pods -> should show one pod for tomcat
 ```
 * Update cluster to 3 nodes (edit tomcat7-controller.json change replica from 1 to 3)
 ```
    $ kubectl update -f tomcat7-controller.json
    $ kubectl get pods -> should show three pods for tomcat
 ```
4. Simple commands
  * Delete a pod
 ```
    $ kubectl delete pod tomcat7-e7hxk
 ```
  * Delete tomcat7 replica controller
 ```
    $ kubectl delete rc tomcat7
 ```
  * Delete tomcat7 service
 ```
    $ kubectl delete services tomcat7
 ```
  * Show all replica controllers
 ```
    $ kubectl get rc 
 ```

 Tomcat visible
  as service @ tomcat7_service_ip:8083
  as pod @ tomcat7_pod_ip:8083

 tomcat7_service_ip can be found by running 'kubectl get services tomcat7'
 Actually there could be multiple tomcat7 pods. To find tomcat7_pod_ip run 'kubectl get pods' and read IP value



[1]:https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/docker.md
[2]:http://sebgoa.blogspot.co.nz/2015/04/1-command-to-kubernetes-with-docker.html
[3]:http://storage.googleapis.com/kubernetes-release/release/v0.18.2/bin/linux/amd64/kubectl
[4]:https://github.com/GoogleCloudPlatform/kubernetes/releases