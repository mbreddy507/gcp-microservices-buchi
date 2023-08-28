Online Boutique

Continuous Integration

Online Boutique is a cloud-first microservices demo application. Online Boutique consists of an 11-tier microservices application. The application is a web-based e-commerce app where users can browse items, add them to the cart, and purchase them.

Google uses this application to demonstrate the use of technologies like Kubernetes, GKE, Istio, Stackdriver, and gRPC. This application works on any Kubernetes cluster, like Google Kubernetes Engine (GKE). It’s easy to deploy with little to no configuration.

If you’re using this demo, please ★Star this repository to show your interest!

Note to Googlers (Google employees): Please fill out the form at go/microservices-demo.

Screenshots
Home Page	Checkout Screen
Screenshot of store homepage	Screenshot of checkout screen
Interactive quickstart (GKE)
Open in Cloud Shell

Quickstart (GKE)
Ensure you have the following requirements:

Google Cloud project.
Shell environment with gcloud, git, and kubectl.
Clone the repository.

git clone https://github.com/GoogleCloudPlatform/microservices-demo
cd microservices-demo/
Set the Google Cloud project and region and ensure the Google Kubernetes Engine API is enabled.

export PROJECT_ID=<PROJECT_ID>
export REGION=us-central1
gcloud services enable container.googleapis.com \
  --project=${PROJECT_ID}
Substitute <PROJECT_ID> with the ID of your Google Cloud project.

Create a GKE cluster and get the credentials for it.

gcloud container clusters create-auto online-boutique \
  --project=${PROJECT_ID} --region=${REGION}
Creating the cluster may take a few minutes.

Deploy Online Boutique to the cluster.

kubectl apply -f ./release/kubernetes-manifests.yaml
Wait for the pods to be ready.

kubectl get pods
After a few minutes, you should see the Pods in a Running state:

NAME                                     READY   STATUS    RESTARTS   AGE
adservice-76bdd69666-ckc5j               1/1     Running   0          2m58s
cartservice-66d497c6b7-dp5jr             1/1     Running   0          2m59s
checkoutservice-666c784bd6-4jd22         1/1     Running   0          3m1s
currencyservice-5d5d496984-4jmd7         1/1     Running   0          2m59s
emailservice-667457d9d6-75jcq            1/1     Running   0          3m2s
frontend-6b8d69b9fb-wjqdg                1/1     Running   0          3m1s
loadgenerator-665b5cd444-gwqdq           1/1     Running   0          3m
paymentservice-68596d6dd6-bf6bv          1/1     Running   0          3m
productcatalogservice-557d474574-888kr   1/1     Running   0          3m
recommendationservice-69c56b74d4-7z8r5   1/1     Running   0          3m1s
redis-cart-5f59546cdd-5jnqf              1/1     Running   0          2m58s
shippingservice-6ccc89f8fd-v686r         1/1     Running   0          2m58s
Access the web frontend in a browser using the frontend's external IP.

kubectl get service frontend-external | awk '{print $4}'
Visit http://EXTERNAL_IP in a web browser to access your instance of Online Boutique.

Once you are done with it, delete the GKE cluster.

gcloud container clusters delete online-boutique \
  --project=${PROJECT_ID} --region=${REGION}
Deleting the cluster may take a few minutes.

Use Terraform to provision a GKE cluster and deploy Online Boutique
The /terraform folder contains instructions for using Terraform to replicate the steps from Quickstart (GKE) above.

Other deployment variations
Istio/Anthos Service Mesh: See these instructions.
non-GKE clusters (Minikube, Kind): see the Development Guide
Deploy Online Boutique variations with Kustomize
The /kustomize folder contains instructions for customizing the deployment of Online Boutique with different variations such as:

integrating with Google Cloud Operations
replacing the in-cluster Redis cache with Google Cloud Memorystore (Redis), AlloyDB or Google Cloud Spanner
etc.
Architecture
Online Boutique is composed of 11 microservices written in different languages that talk to each other over gRPC.

Architecture of microservices

Find Protocol Buffers Descriptions at the ./protos directory.

Service	Language	Description
frontend	Go	Exposes an HTTP server to serve the website. Does not require signup/login and generates session IDs for all users automatically.
cartservice	C#	Stores the items in the user's shopping cart in Redis and retrieves it.
productcatalogservice	Go	Provides the list of products from a JSON file and ability to search products and get individual products.
currencyservice	Node.js	Converts one money amount to another currency. Uses real values fetched from European Central Bank. It's the highest QPS service.
paymentservice	Node.js	Charges the given credit card info (mock) with the given amount and returns a transaction ID.
shippingservice	Go	Gives shipping cost estimates based on the shopping cart. Ships items to the given address (mock)
emailservice	Python	Sends users an order confirmation email (mock).
checkoutservice	Go	Retrieves user cart, prepares order and orchestrates the payment, shipping and the email notification.
recommendationservice	Python	Recommends other products based on what's given in the cart.
adservice	Java	Provides text ads based on given context words.
loadgenerator	Python/Locust	Continuously sends requests imitating realistic user shopping flows to the frontend.
Features
Kubernetes/GKE: The app is designed to run on Kubernetes (both locally on "Docker for Desktop", as well as on the cloud with GKE).
gRPC: Microservices use a high volume of gRPC calls to communicate to each other.
Istio: Application works on Istio service mesh.
Cloud Operations (Stackdriver): Many services are instrumented with Profiling and Tracing. In addition to these, using Istio enables features like Request/Response Metrics and Context Graph out of the box. When it is running out of Google Cloud, this code path remains inactive.
Skaffold: Application is deployed to Kubernetes with a single command using Skaffold.
Synthetic Load Generation: The application demo comes with a background job that creates realistic usage patterns on the website using Locust load generator.
Development
See the Development guide to learn how to run and develop this app locally.

Demos featuring Online Boutique
Use Azure Redis Cache with the Online Boutique sample on AKS
Sail Sharp, 8 tips to optimize and secure your .NET containers for Kubernetes
Deploy multi-region application with Anthos and Google cloud Spanner
Use Google Cloud Memorystore (Redis) with the Online Boutique sample on GKE
Use Helm to simplify the deployment of Online Boutique, with a Service Mesh, GitOps, and more!
How to reduce microservices complexity with Apigee and Anthos Service Mesh
gRPC health probes with Kubernetes 1.24+
Use Google Cloud Spanner with the Online Boutique sample
Seamlessly encrypt traffic from any apps in your Mesh to Memorystore (redis)
Strengthen your app's security with Anthos Service Mesh and Anthos Config Management
From edge to mesh: Exposing service mesh applications through GKE Ingress
Take the first step toward SRE with Cloud Operations Sandbox
Deploying the Online Boutique sample application on Anthos Service Mesh
Anthos Service Mesh Workshop: Lab Guide
KubeCon EU 2019 - Reinventing Networking: A Deep Dive into Istio's Multicluster Gateways - Steve Dake, Independent
Google Cloud Next'18 SF
Day 1 Keynote showing GKE On-Prem
Day 3 Keynote showing Stackdriver APM (Tracing, Code Search, Profiler, Google Cloud Build)
Introduction to Service Management with Istio
Google Cloud Next'18 London – Keynote showing Stackdriver Incident Response Management
