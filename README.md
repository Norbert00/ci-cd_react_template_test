
### Simple CI/CD process for a React application template: 
#### **Jenkins** 
Agent on which the job will run needs to have Docker installed \
**Additional plugins** for Jenkins:  
> nexus-artifact-uploader \
 Active Choices

#### **Sonatype nexus repository** 
How to setup using docker: 
>https://ahgh.medium.com/how-to-setup-sonatype-nexus-3-repository-manager-using-docker-7ff89bc311ce

#### In Jenkinsfile change the environment variables
**NEXUS_URL** - specifying either the URL or the IP address of the nexus \
**NEXUS_REPOSITORY** - the name of the nexus repository
