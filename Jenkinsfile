pipeline {
    agent any

    tools {
        maven 'Maven' // The name of the Maven tool configured in Jenkins
    }

    environment {
        DOCKER_IMAGE = "your-dockerhub-username/spring-boot-cicd"
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds' // Jenkins credentials ID for DockerHub
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/spring-boot-cicd.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        docker.image(DOCKER_IMAGE).push("latest")
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying to Kubernetes...'
                sh '''
                kubectl apply -f k8s/deployment.yaml
                kubectl apply -f k8s/service.yaml
                '''
            }
        }
    }
}

