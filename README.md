# React Application with Docker and AWS Elastic Beanstalk

## 1. Why Use Elastic Beanstalk over ECS or EC2?

**Elastic Beanstalk** is a Platform-as-a-Service (PaaS) solution that simplifies deploying and managing applications on AWS. Here's why you might choose **Elastic Beanstalk** over **Amazon ECS** (Elastic Container Service) or **EC2** (Elastic Compute Cloud):

- #### **Simplified Management**
##### **Elastic Beanstalk** handles infrastructure management for you. It takes care of provisioning, load balancing, scaling, and monitoring, allowing you to focus on building your application rather than managing the underlying infrastructure.

- #### **Automatic Scaling**
##### **Elastic Beanstalk** automatically scales your application up or down based on demand, without requiring manual configuration of Auto Scaling groups as you would in **EC2** or **ECS**.

- #### **Integrated CI/CD**
##### **Elastic Beanstalk** easily integrates with Continuous Integration/Continuous Deployment (CI/CD) tools like **Travis CI**, automating the deployment process with minimal manual intervention.

- #### **Less Operational Overhead**
##### **Elastic Beanstalk** abstracts much of the operational complexity compared to **EC2** or **ECS**. You don’t need to manage underlying EC2 instances or configure ECS clusters. It’s a fully managed service, reducing the need for in-depth infrastructure knowledge.

- #### **Docker Support**
##### **Elastic Beanstalk** natively supports Docker containers, enabling you to deploy containerized applications with minimal setup.

- #### **Managed Service**
##### **Elastic Beanstalk** is a fully managed service that provides automatic updates, health checks, and monitoring, so you don’t need to worry about system maintenance like with **EC2** or **ECS**.

- #### **Faster Deployment**
##### **Elastic Beanstalk** simplifies the deployment process, allowing faster deployment of web applications. It requires less configuration compared to setting up **ECS** clusters or **EC2** instances.

- #### **Cost-Effective for Small Teams**
##### For smaller teams or startups, **Elastic Beanstalk** helps reduce the complexity and administrative overhead, which can save both time and costs compared to managing **EC2** or **ECS** manually.

## **Use Elastic Beanstalk**
If you're looking for a simplified and managed platform for deploying your application with minimal operational overhead, **Elastic Beanstalk** is the perfect solution. It’s ideal for teams who want to focus on their code and product rather than spending time managing infrastructure. Use **Elastic Beanstalk** for streamlined deployments, automatic scaling, and reduced complexity in your development process.

## 2. Technologies Used

- **React** - JavaScript library for building user interfaces.
- **Docker** - Platform for developing, shipping, and running applications inside containers.
- **Docker-compose** - Tool for defining and running multi-container Docker applications.
- **AWS Elastic Beanstalk** - Platform-as-a-Service (PaaS) for deploying and managing applications in the cloud.
- **TravisCI** - Continuous Integration and Continuous Deployment (CI/CD) tool for automating the deployment process.

## Project Overview

This project is built with a basic **React** application, but the focus is primarily on **infrastructure**. The project is set up using **Docker** to provide a reliable and isolated development environment. **Docker-compose** is used to manage multiple containers, and the application is deployed to **AWS Elastic Beanstalk**. **TravisCI** is configured to automate deployment and ensure the application is always up-to-date.

## 3. Getting Started

To get started with this project locally, follow the steps below:

### 1. Clone the repository

```bash
git clone <repository-url>
cd <repository-name>
```

### 2. Set up the environment

To run the React application locally, you need to use the `Dockerfile.dev`. This Dockerfile sets up a development environment where changes you make to the code are reflected in real-time.

### 3. Run Docker Compose

Use **Docker Compose** to set up and run your development environment. In the root of the project, run:

```bash
docker-compose -f docker-compose-dev.yml up
```

This will start the React application in a container, and you'll be able to view the changes to the code in real-time without needing to rebuild the container every time you make changes.

### 4. Access the Application

Once the Docker container is running, open your browser and go to `http://localhost:3000` to see the React application running locally.

### 5. Real-time Code Updates

The `Dockerfile.dev` is configured to watch for changes in the code, so as soon as you update any of your files, those changes will be reflected in the application in real time. You don’t need to restart the Docker container.

## 4. Working with AWS Elastic Beanstalk

For deploying this application to AWS Elastic Beanstalk, make sure you have your AWS credentials set up and your environment is configured to use Elastic Beanstalk.

1. **Create a new Elastic Beanstalk environment**: 
   If you don't have an environment set up already, you can use the AWS Console or CLI to create one. This will handle the provisioning of resources such as EC2 instances, load balancers, and auto-scaling.
   
### Create EC2 IAM Instance Profile

1. Go to the **AWS Management Console**.
2. In the search bar, type **IAM** and click the **IAM** service.
3. In the left sidebar, click **Roles** under **Access Management**.
4. Click the **Create role** button.
5. Select **AWS Service** under **Trusted entity type**, then select **EC2** under **common use cases**.
6. Search for **AWSElasticBeanstalk** and select the following policies:
   - **AWSElasticBeanstalkWebTier**
   - **AWSElasticBeanstalkWorkerTier**
   - **AWSElasticBeanstalkMulticontainerDocker**
7. Click the **Next** button.
8. Give the role the name: `aws-elasticbeanstalk-ec2-role`.
9. Click the **Create role** button.

### Create Elastic Beanstalk Environment

1. Go to the **AWS Management Console**.
2. In the search bar, type **Elastic Beanstalk** and click the **Elastic Beanstalk** service.
3. If this is your first time using Elastic Beanstalk, you will see a splash page. Click the **Create Application** button. If you've used Elastic Beanstalk before, you will be taken directly to the Elastic Beanstalk dashboard. In this case, click the **Create environment** button. There are 6 steps in the flow you will follow.
4. Provide an **Application name**, which will automatically populate an **Environment Name**.
5. Scroll down to the **Platform** section and select **Docker** as the platform. This will auto-select several default options. Change the **Platform branch** to **Docker running on 64bit Amazon Linux 2** (Note: The new 2023 branch has issues with single-container deployments).
6. Scroll down to the **Presets** section and ensure **free tier eligible** is selected.
7. Click the **Next** button to proceed to Step #2.
8. In the **Service Access** configuration form.
Select Create and use new service role and name it aws-elasticbeanstalk-service-role. You will then need to set the EC2 instance profile to the aws-elasticbeanstalk-ec2-role created earlier (this will likely be auto-populated for you).
10. Click the Skip to Review button as Steps 3-6 are not applicable.
11. Click the Submit button and wait for your new Elastic Beanstalk application and environment to be created and launch.
12. Click the link below the checkmark under Domain. This should open the application in your browser and display a Congratulations message.


## 5. **Deploy to Elastic Beanstalk**: 
   The application is configured for deployment via **TravisCI**. It automatically pushes the changes to **Elastic Beanstalk** whenever you push to the `main` branch. Check `.travis.yml` file and add your env information. Add your `Environment Variables` in settings of your Travis.ci repository.

## 6. **Continuous Integration with TravisCI**: 
   When you push changes to the `main` branch, **TravisCI** will automatically trigger a build and deployment process, ensuring that your app is always up-to-date in the cloud.


# Write your React application on a ready-to-use infrastructure — good luck and happy coding! <3


