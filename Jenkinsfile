pipeline {
    agent any

    environment {
        APP_NAME = "rohith-web-app"
        DOCKER_IMAGE = "rohith-web-app-image"
        DOCKER_CONTAINER = "rohith-web-container"
    }

    tools {
        maven 'M3'   // Make sure Maven is configured in Jenkins
        jdk 'java'      // Adjust based on your setup
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/your-username/your-repo.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop $DOCKER_CONTAINER || true
                docker rm $DOCKER_CONTAINER || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker run -d -p 8080:8080 --name $DOCKER_CONTAINER $DOCKER_IMAGE
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build and Deployment Successful!'
        }
        failure {
            echo '❌ Build Failed!'
        }
    }
}
