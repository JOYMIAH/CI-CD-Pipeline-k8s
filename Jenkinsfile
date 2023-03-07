pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
				git branch: 'main', credentialsId: 'githubtoken1', url: 'https://github.com/JOYMIAH/CI-CD-Pipeline-k8s.git'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
					echo 'Buid Docker Image'
                    docker build -t joymiah1/todo_app:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'login into dockerhub'
                    docker login -u joymiah1 -p Joy--4108
                    echo 'Push to the dockerhub'
                    docker push joymiah1/todo_app:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
				git branch: 'main', credentialsId: 'githubtoken1', url: 'https://github.com/JOYMIAH/Deployment_ManifestFile.git'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                        sh '''
						ls -lrt
						pwd
                        cat deploy.yaml
                        sed "s/IMAGE_TAG/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
						git remote add test https://github.com/JOYMIAH/Deployment_ManifestFile.git
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git push --force test  main
                        '''                        
                    }
                }
            }
        }
    }
