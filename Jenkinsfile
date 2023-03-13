pipeline {
    
    agent any 
    
    environment {
        dockerTag = "joymiah1/todo_app:${env.BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                git branch: 'main', credentialsId: 'GitHubCredentials', url: 'https://github.com/JOYMIAH/CI-CD-Pipeline-k8s.git'
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
                    echo 'Push to Repo'
                    docker push joymiah1/todo_app:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git branch: 'main', credentialsId: 'GitHubCredentials', url: 'https://github.com/JOYMIAH/deployment.git'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                     git branch: 'main', credentialsId: 'GitHubCredentials', url: 'https://github.com/JOYMIAH/deployment.git'
                        sh '''
                        cat deploy.yaml
                        ls -lrt
                        sed -i 's|joymiah1/todo_app:.*|${dockerTag}|g' deploy.yaml
                        pwd
                        git init
                        git add .
                        ls -lrt
                        git commit -m "Test"
                        git push https://github.com/JOYMIAH/deployment.git
                        '''                        
                    }
                }
            }
        }
    }
