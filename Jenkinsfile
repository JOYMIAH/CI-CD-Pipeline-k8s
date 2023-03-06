	pipeline {
	    agent any
	    environment{
	        DOCKER_TAG = getDockerTag()
	    }
	    stages{
	        stage('Build Docker Image'){
	            steps{
	                sh "docker build . -t joymiah1/todo_app:${DOCKER_TAG}"
	            }
	        }
	        stage('Dockerhub push'){
	            steps{
	                    sh "docker login -u joymiah1 -p -p Joy--4108"
	                    sh "docker push joymiah1/todo_app:${DOCKER_TAG}"
	                    }
	            }   
	        }
	        stage('Deploy to kubernetes'){
	            steps{
	                sh "chmod +x script.sh"
	                sh "./script.sh ${DOCKER_TAG}"
	            }
	        }
	    }

	

	def getDockerTag(){
	    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
	    return tag
	}
