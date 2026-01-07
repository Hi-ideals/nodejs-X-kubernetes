pipeline {
    agent any

    environment {
        REGISTRY = "docker.io/jaicswamy962"
        FRONTEND_IMAGE = "${REGISTRY}/frontend:v1"
        BACKEND_IMAGE  = "${REGISTRY}/backend:v1"
    }

    stages {
        
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Hi-ideals/nodejs-X-kubernetes.git'
                sh 'cd nodejs-X-kubernetes'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh '''
                docker build -t $FRONTEND_IMAGE frontend/
                docker build -t $BACKEND_IMAGE backend/
                '''
            }
        }

        stage('Build Docker Images') {
            steps {
                sh '''
                docker build -t $FRONTEND_IMAGE ./frontend
                docker build -t $BACKEND_IMAGE ./backend
                '''
            }
        }

        stage('Push Docker Images') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $FRONTEND_IMAGE
                    docker push $BACKEND_IMAGE
                    docker logout
                    '''
                }
            }
        }

        stage('Deploy Database') {
            steps {
                sh 'kubectl apply -f database/'
            }
        }

        stage('Deploy Backend') {
            steps {
                sh 'kubectl apply -f backend/'
            }
        }

        stage('Deploy Frontend') {
            steps {
                sh 'kubectl apply -f frontend/'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                kubectl get pods
                kubectl get svc
                '''
            }
        }
    }
}
