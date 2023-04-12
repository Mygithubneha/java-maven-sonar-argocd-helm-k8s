pipeline {
    agent {
        docker {
            image 'abhishekf5/maven-abhishek-docker-agent:v1'
            args '--user root -v /var/run/docker.docker.sock:/var/run/docker.sock' // mount Docker socket to access the host's Docker daemon
        }
    }
    stages {
        stage('Checkout') {
           steps {
              sh 'echo passes'
              git branch: 'main', url: 'https://github.com/Mygithubneha/java-maven-sonar-argocd-helm-k8s.git'
           }
        }
    }

        stage('Build & Test') {
            steps {
                sh 'ls -ltr'
                //build the project and create a JAR file
                sh 'cd java-maven-sonar-argocd-helm-k8s/spring-boot-app && mvn clean package'
            }
    }
}