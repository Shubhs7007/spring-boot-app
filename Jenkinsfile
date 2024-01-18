pipeline {
    agent { label 'Jenkins Agent' }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
	    APP_NAME = "spring-boot-app-pipeline"
            sudo RELEASE = "1.0.0"
            sudo DOCKER_USER = "shubhs7007"
            sudo DOCKER_PASS = 'dockerhub'
            sudo IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            sudo IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/Shubhs7007/spring-boot-app.git'
                }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }

       stage("SonarQube Analysis"){
           steps {
	           script {
		        withSonarQubeEnv(credentialsId: 'Jenkins-Sonarqube-Tokan') { 
                        sh "mvn sonar:sonar"
		        }
	           }	
           }
       }
	stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                       sudo docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                       sudo docker_image.push("${IMAGE_TAG}")
                        sudo docker_image.push('latest')
		    }
		}
	    }
	}
    }
}


 
      
