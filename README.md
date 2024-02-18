Pipeline README
This pipeline script is designed to automate the build, packaging, and deployment process of a Java application along with creating and pushing a Docker image to a Docker Hub repository. The pipeline is written in Groovy and utilizes Jenkins declarative pipeline syntax.

Prerequisites
Jenkins server with the Pipeline plugin installed
Maven installed on the Jenkins server
Docker installed on the Jenkins server
Access to a Docker Hub account for pushing Docker images
Access to an EC2 instance for deploying the Docker image
Pipeline Structure
The pipeline consists of the following stages:

Agent: Specifies that this pipeline can run on any available agent.
Tools: Configures the Maven tool named 'Maven' to be used within the pipeline.
Stages:
build app: Builds the Java application using Maven.
build image: Builds the Docker image for the application and pushes it to a Docker Hub repository.
 <!-- - **deploy**: Deploys the Docker image to an EC2 instance. -->
Usage
Jenkins Configuration:

Ensure Jenkins is properly configured with necessary credentials:
Docker Hub credentials (username and password) with ID 'walid-docker-hub-repo'.
  <!-- - SSH private key credential with ID 'ec2-server-key' for accessing the EC2 instance. -->
Install required plugins: Pipeline, Maven Integration, Docker Pipeline.
Pipeline Setup:

Create a new pipeline job in Jenkins.
Select "Pipeline script" and copy-paste the provided Groovy script into the pipeline script editor.
Pipeline Execution:

When triggered, Jenkins will execute the pipeline stages sequentially.
Ensure that Maven dependencies are correctly specified in the pom.xml file.
Modify Docker image naming and versioning as per project requirements.
 <!-- - Enable the deploy stage and configure it with the appropriate commands for deploying to your EC2 instance. -->
Monitoring:

Monitor the Jenkins job console output for any errors or warnings during the pipeline execution.
 <!-- - Monitor the EC2 instance for successful deployment and application availability. -->
Notes
Portions of the pipeline script have been commented out for stages related to version incrementation and EC2 deployment. Uncomment and customize these stages according to your project requirements.
Ensure proper security measures are in place, especially when handling credentials and deploying to production environments.
Feel free to customize this pipeline script according to your specific project needs and infrastructure setup. If you encounter any issues or have questions, please refer to the Jenkins documentation or seek assistance from your DevOps team.
