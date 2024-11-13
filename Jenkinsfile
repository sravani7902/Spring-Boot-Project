pipeline {
    agent any

    stages {
      
       stage('Build') {
            steps {
                script {
                    // Use Maven to build the project
                    sh 'mvn clean package'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run unit tests using Maven
                    sh 'mvn test'
                }
            }
        }

    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
