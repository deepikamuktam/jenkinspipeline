-- Create an EC2 instance and install and configure jenkins

sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a
/etc/apt/sources.list.d/trivy.list
sudo apt-get update

-- Create a Project:
Implement a CICD pipeline in Declarative format

-- Create a project
Jenkins Dashboard —> New item —> give project a name → project type (Pipeline)

<img width="1291" height="635" alt="Screenshot 2025-08-06 112206" src="https://github.com/user-attachments/assets/5e025058-a1da-4f3c-9c63-0e728cbd3475" />

-- Declarative pipeline:

pipeline {
    agent any

    triggers {
        pollSCM('* * * * *') // Poll SCM every minute for changes
    }

    tools {
        maven 'Maven 3.8.8' // Must match the Maven name configured in Jenkins
        jdk 'Java 17'       // Must match the JDK name configured in Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/nocturnaldevops/Project1'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }

        // Optional deploy stage
        // stage('Deploy') {
        //     steps {
        //         echo 'Deploying the application...'
        //     }
        // }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Build was successful.'
        }
        failure {
            echo 'Build failed.'
        }
    }
}

-- save the pipeline 
-- build the job 
