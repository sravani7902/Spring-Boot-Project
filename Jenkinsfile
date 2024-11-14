pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sravani7902/first-jenkins"
        GITHUB_CREDENTIALS = credentials('github-personal-token')
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-access-token')
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository from GitHub...'
                git branch: 'master', url: 'https://github.com/sravani7902/Spring-Boot-Project.git', credentialsId: 'github-personal-token'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'mvn clean test'
            }
        }

        stage('Package') {
            steps {
                echo "Packaging the application..."
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            when {
                expression { return currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                echo 'Building Docker image...'
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Deploy to Server') {
            when {
                expression { return currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                echo 'Deploying Docker image...'
                sh 'docker login -u $DOCKERHUB_CREDENTIALS_USR -p $DOCKERHUB_CREDENTIALS_PSW'
                sh 'docker push ${DOCKER_IMAGE}:latest'
            }
        }
    }
}
