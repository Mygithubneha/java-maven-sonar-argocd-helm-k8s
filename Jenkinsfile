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

        stage('Build & Test') {
            steps {
                sh 'ls -ltr'
                //build the project and create a JAR file
                sh 'mvn clean package'
            }
        }

        stage('Static Code Analysis') {
            environment {
                SONAR_URL = "http://65.2.6.217:9000"
            }
            steps {
                withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
                    sh 'mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
                     
                }
            }       
        }

        stage('Build & Push Docker Image') {
            environment {
                DOCKER_IMAGE = "mydockerhub121/ultimate-cicd:${BUILD_NUMBER}"
                //DOCKERFILE_LOCATION = "./Dockerfile"
                DOCKER_REGISTRY_CREDENTIALS = credentials('dockerhub')
            }
            steps {
                script {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                    def dockerImage = docker.image("${DOCKER_IMAGE}")
                    docker.withRegistry('https://index.docker.io/v1/', "dockerhub") {
                    dockerImage.push()
                    }
                }
                
            }
        }
    }
}
       
    
