Muhammad Azmy Arya Rizaldi M. - 2206081704

# Reflections
1. ***Compare the application logs before and after you exposed it as a Service. Try to open the app several times while the proxy into the Service is running. What do you see in the logs? Does the number of logs increase each time you open the app?***

From the logs, it is clear that before exposing the application as a service, it was accessed directly within the pod. The logs show initial messages (start server HTTP on port 8080) and each incoming request (GET /). Each time the application is accessed within the pod, a log entry is created for that request. After exposing the application as a service using minikube service hello-node, the logs still show the initial messages and incoming requests as before, but now the application is accessed through the service, which forwards the traffic to the pod. Accessing the application through the service enables external access to the application, while accessing it within the pod is more internal to the Kubernetes cluster.

<img width="878" alt="Screenshot 2024-05-17 at 21 02 55" src="https://github.com/azmyar/tutorial11-kube/assets/119117836/ef28a009-261e-4141-9756-63ec05e21593">

3. ***Notice that there are two versions of kubectl get invocation during this tutorial section. The first does not have any option, while the latter has -n option with value set to `kube-system`. What is the purpose of the -n option and why did the output not list the pods/services that you explicitly created?***

The -n option in the kubectl get command is used to specify the namespace where the resources will be displayed. If the -n option is not included, kubectl get will display resources from the default namespace. In the latter invocation, kubectl get is used with the -n kube-system option, which instructs it to display resources specifically from the kube-system namespace. This namespace contains the system components and infrastructure of Kubernetes itself, such as core system services like DNS, the API server, and the Kubernetes dashboard.

# Rolling Update & Kubernetes Manifest File

1. **What is the difference between Rolling Update and Recreate deployment strategy?**

The difference between the Rolling Update and Recreate deployment strategies lies in how they manage application updates and the associated downtime. In a Recreate Deployment, downtime occurs because all pods from the previous version of the application are stopped before new pods with the latest version are created. This means that during the period between the deletion of the old pods and the creation of the new ones, the application is unavailable. On the other hand, Rolling Update avoids downtime by performing the update gradually. New pods with the latest version of the application are created one by one while the old pods continue to run. Thus, Rolling Update ensures that the application remains online throughout the update process with no downtime.

2. **Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt.**

To apply the Recreate Deployment Strategy, I used a manifest file with a different strategy type and used the same service manifest file as in the previous tutorial.
<img width="1679" alt="Screenshot 2024-05-17 at 22 39 25" src="https://github.com/azmyar/tutorial11-kube/assets/119117836/ec3b4155-ca89-4d48-8f37-7dd2007c3994">
<img width="1668" alt="Screenshot 2024-05-17 at 22 39 44" src="https://github.com/azmyar/tutorial11-kube/assets/119117836/e67b7e30-b43f-4b3a-8798-484fc72d9bbb">
<img width="1677" alt="Screenshot 2024-05-17 at 22 39 59" src="https://github.com/azmyar/tutorial11-kube/assets/119117836/dc8dda11-b538-4d94-a881-a450d3e6fefb">

3. **Prepare different manifest files for executing Recreate deployment strategy.**

I named the manifest file recreate-deployment.yaml, based on the deployment.yaml from the tutorial. I modified it by changing the strategy type to Recreate. The recreate-deployment.yaml is pushed to this repo.

4. **What do you think are the benefits of using Kubernetes manifest files? Recall your experience in deploying the app manually and compare it to your experience when deploying the same app by applying the manifest files (i.e., invoking kubectl apply -f command) to the cluster.**

Using manifest files standardizes the deployment procedure and documents it declaratively, reducing the risk of human error as there is no need to remember specific steps. Manifest files enable efficient application configuration management, eliminating concerns about time-consuming manual setups.