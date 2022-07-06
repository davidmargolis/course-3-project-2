# Deployment of Java Application on Tomcat Apache Using Pipeline - Project 2 

## DESCRIPTION

Use Jenkins to deploy Java Application to Tomcat Apache

### Description:

While developing a highly scalable application, real challenges come into picture during the deployment of that application into production or real time data scenarios. One of the biggest drawbacks for any product after development is having the application stuck while working in normal case scenarios in a real time production environment.

Your organization is adopting modern DevOps processes to automate deployment on Tomcat Apache using Jenkins CI/CD pipelines.

Jenkins’s pipeline using Groovy DSL language makes easy to integrate build, test and deployment stages within single pipeline. Considering this as urgent requirement, organization is focusing on automating build and deployment on Tomcat Apache. Pipeline should be designed in such a way that it would be triggered automatically without any developer’s interaction.

__Tools required__: Git, GitHub, Jenkins, Tomcat Apache and Maven

### Expected Deliverables:

Configure Maven Build on Build Server for automating builds.
Integrate Maven build tool and then perform test cases execution and deployment on Tomcat Apache.

## Solution

### Setup Github

- [Create course-3-project-2 repo](https://github.com/davidmargolis/course-3-project-2) in Github
- [Generate personal access token](https://github.com/settings/tokens/new) for integrating jenkins

### Setup Jenkins in AWS Practice Lab

1. Open and start [practice labs](https://caltech.lms.simplilearn.com/courses/4041/-PG-DO---CI%2FCD-Pipeline-with-Jenkins/practice-labs)
1. Open [aws](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1#)
1. Create AWS Architecture
    1. [Create key pair](https://us-east-1.console.aws.amazon.com/ec2/v2/home?region=us-east-1#CreateKeyPair:):
        - name - `jenkins_on_ec2`
        - Private key file format - `pem`
    1. [Create security group](https://us-east-1.console.aws.amazon.com/ec2/v2/home?region=us-east-1#CreateSecurityGroup:):
        - Security group name - `WebServerSG`
        - Description - `Jenkins`
        - Add Inbound Rules:
            - Rule 1:
                - Type - `SSH`
                - Source - `My IP`
            - Rule 2:
                - Type - `HTTP`
                - Source - `Anywhere-IPv4`
            - Rule 3:
                - Type - `Custom TCP`
                - Port range - `8080`
                - Source - `Anywhere-IPv4`
    1. [Create ec2 instance](https://us-east-1.console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstances:):
        - Instance type pair name - `jenkins_on_ec2`
        - Key pair name - `jenkins_on_ec2`
        - Select <input type=radio checked> `existing security group`
        - Common security groups Info - `WebServerSG`
1. Install Jenkins in EC2 instance
    1. Shell into ec2:
        ```
        mv /mnt/c/Users/david/Downloads/jenkins_on_ec2.pem ~/.ssh \
        && chmod 400 ~/.ssh/jenkins_on_ec2.pem \
        && ssh -i ~/.ssh/jenkins_on_ec2.pem ec2-user@ec2-54-165-149-225.compute-1.amazonaws.com
        ```
    1. Install git, java 11, and jenkins on EC2
        ```
        sudo yum update –y \
            && sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo \
            && sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key \
            && sudo amazon-linux-extras install java-openjdk11 -y \
            && sudo yum install git jenkins -y \
            && sudo systemctl enable jenkins \
            && sudo systemctl start jenkins \
            && sudo systemctl status jenkins
        ```
    1. Get jenkins password:
        ```
        cat /var/lib/jenkins/secrets/initialAdminPassword
        ```
    <!-- 1. Install git and maven
        ```
        sudo yum update -y \
            && sudo yum install git maven -y
        ``` -->
1. Configure Jenkins
    1. Log into Jenkins <http://ec2-54-165-149-225.compute-1.amazonaws.com:8080/>
    1. Choose `Install standard plugins`
    1. Add GitHub Server
        1. [Configure System](http://ec2-54-165-149-225.compute-1.amazonaws.com:8080/configure)
        1. Click `Add GitHub Server` -> `GitHub Server`
        1. Click `Save`
    1. [Create new job](http://ec2-54-165-149-225.compute-1.amazonaws.com:8080/view/all/newJob)
        1. `Enter an item name` - `course-3-project-2`
        1. Select `Freestyle project`
        1. Click `OK`
        1. Check <input type="checkbox" checked> `Discard old builds`
        1. Check <input type="checkbox" checked> `GitHub project`
        1. `Project url` - <https://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@github.com/davidmargolis/course-3-project-2.git>
        1. Choose `Source Code Management` <input type="radio" checked> `Git`
        1. `Repository URL` - <https://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@github.com/davidmargolis/course-3-project-2.git>
        1. Check <input type="checkbox" checked> `GitHub hook trigger for GITScm polling`
        1. Click `Save`
    1. [Add webhook](https://github.com/davidmargolis/course-3-project-2/settings/hooks/new) in GitHub:
        1. `Payload URL` - <http://ec2-54-165-149-225.compute-1.amazonaws.com:8080/github-webhook/>
        1. `Content type` - `application/type`

### Run Jenkins Pipeline

1. Commit a change to git repo to trigger pipeline
