# Introduction

This is a simple blog microservice project built with Node.js, Express.js, and React.js. 

The project is finished by following the instructions provided by an Udemy course **Microservice with Node.js, React, Dokcer, and Kubernetes.**

Users are able to submit posts and comments on the blog. All the posts and comments are stored in memory of the server of “query” service. Therefore, when the server terminates, the posts and comments records will be gone.

The communication between each service is triggered by events.

All services are containerized, and managed by K8s deployments. Additionally, all services are exposed as network services with K8s Service. Nginx Ingress is used as the ingress to access different services that are in different pods in the K8s node. 

The deployment streamline of pods, deployments, and services are automated by **Skaffold**.

# Core Services

1. **Clients service**: This service manages the user interfaces and integrates all the features.
2. **Posts service**: This service manages to create a post and emits a `PostCreated` event to the Event-bus service.
3. **Comments service**: This service allows users to leave comments under different posts. Once a comment is left, comments service emits a `CommentCreated` event to the Event-bus service. If Comments service received `CommentModerated` events sent from the Moderate service, Comments service will emit a `CommentUpdated` event to the Event-bus service.
4. **Event-bus service**: This service manages to store all the events and emits events to Post service, Comments service, Query service, and Moderate service. Event-bus service is like a router for all the events; communication between services is done by this service.
5. **Moderate service**: This service manages to moderate the comments that have been uploaded and emits `CommentModerated` event. If a comment contains the word “orange”, the comment will be labeled as rejected and will not be displayed. Otherwise, the comment will be labeled as approved and displayed on the webpage.
6. **Query service**: This service manages to integrate posts and comments into a new data structure so that one request will be enough to retrieve all the posts and comments that are stored in the server. By implementing this service, efficiency can be maximized when there are a large number of posts and comments.

# To Start the Project
```bash
# Step1. Please set 127.0.0.1 as posts.com in your local hosts file.
# Step2. In the root directory (Blog)
$ skaffold dev

#Step3. Enter posts.com in your browser
```


