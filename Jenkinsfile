pipeline {
    agent any
	
    tools {
        maven 'mvn-3.6.3' 
    }
    environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
	DOCKERHUB_CREDENTIALS=credentials('dockerhub-credentials')
	    
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
                sh 'docker image build -t imransetiadi22/hello-world-maven:1.0 .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing image..'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push imransetiadi22/hello-world-maven:1.0'
            }
        }
        stage('Deploy'){
            steps {
                sh "docker stop hello-world | true"
                sh "docker rm hello-world | true"
                sh "docker run --name hello-world -d -p 9004:8080 vigneshsweekaran/hello-world:${TAG}"
            }
        }
    }
}
