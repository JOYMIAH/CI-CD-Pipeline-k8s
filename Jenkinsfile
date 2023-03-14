pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
		git branch: 'main', url: 'https://github.com/JOYMIAH/CI-CD-Pipeline-k8s.git'
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
                    echo 'Push to the dockerhub'
                    docker push joymiah1/todo_app:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                        sh '''
			ls -lrt
			pwd
			echo "hello" >> joy.txt
                        cat deploy.yml
                        sed "s/55/${BUILD_NUMBER}/g" deploy.yml
                        cat deploy.yml
			git config --global user.email "shaharianazimjoy@gmail.com"
			git config --global user.name "JOYMIAH"
			git remote add joy2 https://github.com/JOYMIAH/Deployment_ManifestFile.git
                        git add .
                        git commit -m 'Updated the deploy yml | Jenkins Pipeline'
                        git push --force test  main
                        '''                        
                    }
                }
            }
        }
    }
