# Introduction spring-ai-chat

The spring-ai-chat app provides you an out-of-the-box application setup to fast start development of a Web Application for AI Chat based on [Spring AI](https://spring.io/projects/spring-ai).

This web application is using Spring AI to offer an interactive chat experience utilizing RAG (Retrieval Augmented Generation) to enable a user to ask questions about their own uploaded documents.

## Prerequisites
In order to further develop this application the following tools needs to be setup:
- Java Development Kit (https://bell-sw.com/)
- Visual Studio Code or IntelliJ IDEA as Integrated Development Environment (IDE)
- Tanzu Developer Tools plugin for the mentioned IDE

# Local

## Build
In order to compile the production code:

```sh
./mvnw clean compile
```

After that it is a good habit to compile the test classes and execute those tests to see if your application is still behaving as you would expect:

```sh
./mvnw verify
```

## Start and interact
Spring Boot has its own integrated Web Server (Apache Tomcat (https://tomcat.apache.org/)).

Set the `AI_API_KEY` environment variable with the API key to be used by your app:

```sh
export AI_API_KEY='<your-api-key>'
```

Launch application using default profile:

```sh
./mvnw spring-boot:run
```

### Accessing home page

You can access the public page at `http://localhost:8080/` by a web browser or using `curl`:

```sh
curl -L -H 'Content-Type: application/html' http://localhost:8080/
```

You'll be presented with a login page. You may login with either of the following sets
of credentials:

 - buzz / infinity
 - woody / bullseye

The security around the application is primarily so that each user will have their own,
distinct chat history and so that conversations with the LLM do not bleed into each
other.

# Deployment

## Dependencies
1. Tanzu CLI and the apps plugin v0.2.0 which are provided as part of [Tanzu Platform](https://docs.vmware.com/en/VMware-Tanzu-Platform/index.html). Installation instructions can be found at [ VMware Tanzu Platform Product Documentation - Before you begin](https://docs.vmware.com/en/VMware-Tanzu-Platform/SaaS/create-manage-apps-tanzu-platform-k8s/getting-started-deploy-app-to-space.html#before-you-begin-0).

2. You have access to a space for your project and you have used `tanzu login` to authenticate and configure your current Tanzu context and set your project and space using `tanzu project use` and `tanzu space use` respectively.

## Configuring your app environment

Change to the root directory of your generated app.

### About the ContainerApp

The project contains a `ContainerApp` manifest file that can be used when building and deploying the app. To review the content of this file run:

```sh
cat .tanzu/config/spring-ai-chat.yml
```

### Configure HTTP Ingress Routing

If want to expose your application with a domain name and route traffic from the domain name to the deployed application, see [Adding HTTP Routing to an Application](https://docs.vmware.com/en/VMware-Tanzu-Platform/SaaS/create-manage-apps-tanzu-platform-k8s/how-to-ingress-to-app.html).

### Configure an image registry to use for the ContainerApp

```sh
tanzu build config --containerapp-registry REGISTRY
```

> Where `REGISTRY` is your container image registry location. For example, `my-registry.io/my-corp-apps/{name}` or `docker.io/<your-docker-id>/{name}` if you use Docker Hub. Note that use of literal `{name}` is required and it will be replaced with your apps name when you build.

## Deploying and building in one step

Change to the root directory of your generated app.

Run this command to build and deploy the app:

```sh
tanzu deploy
```

## Using separate build and deploy commands

Change to the root directory of your generated app.

### Building with local source

You can build using source from a locally cloned Git repository or from source on your local disk.

To build the app you can run this command:

```sh
tanzu build --output-dir ./prebuilt
```

### Deploying the sample on Tanzu Platform for Kubernetes

Start the app deployment by running:

```sh
tanzu deploy --from-build ./prebuilt
```

### Configure the AI API key

The application requires an AI API key to be provided.

You can set an environment variable for the app using this command:

```sh
tanzu app env set spring-ai-chat AI_API_KEY=<your-api-key>
```

### Scale the number of instances

Run this command to scale to 1 instance

```sh
tanzu app scale spring-ai-chat --instances=1
```

### PostgreSQL/pgvector

If you chose PostgreSQL/pgvector as your vector store, you'll need to create
a PostgresSQL database instance before deploying the application. The PostgreSQL database needs to have the "vector" extension available.

Instructions TBD.

# How to proceed from here?

Having the application locally running and deployed to a cluster you could add your domain logic, related persistence and new Spring MVC controllers.

Some tips:
- You can add images, additional CSS, etc to `src/main/resources/static` folder. It will be served by Spring Boot under `/static`. Those resources can be referenced to by Thymeleaf `@` character.
- In order to add a new page, create a new Controller, method and .html file in `src/main/resource/template` folder.

# References
- [Spring Boot](https://spring.io/projects/spring-boot/)
- [Spring AI](https://spring.io/projects/spring-ai)
- [Tanzu Application Platform](https://tanzu.vmware.com/application-platform)
- [Tanzu Developer Tools for Visual Studio Code](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.2/tap/GUID-vscode-extension-about.html)
- [Tanzu Developer Tools for IntelliJ](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.2/tap/GUID-intellij-extension-about.html)
- [Thymeleaf](https://www.thymeleaf.org/)
