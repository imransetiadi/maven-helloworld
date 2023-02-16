pipeline {
    agent any
	
    tools {
        maven 'mvn-3.6.3' 
    }
    environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
	DOCKERHUB_CREDENTIALS=credentials('dockerhub-credentials')
        DOCKER_HUB_REPO = "imransetiadi22/hello-world-maven
    }
    stages {
        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                echo 'Building..'
                sh 'docker image build -t $DOCKER_HUB_REPO:${TAG} .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing image..'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push $DOCKER_HUB_REPO:${TAG}'
            }
        }
        stage('Deploy to Cluster Kubernetes') {
            steps {
                echo 'Deploying....'
                sh 'sudo kubectl apply -f deployment.yaml'
                sh 'sudo kubectl apply -f service.yaml'
            }
        }
    }
}
