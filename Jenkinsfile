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
                    echo 'Push to Repo'
                    docker push joymiah1/todo_app:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/JOYMIAH/deployment.git'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                        sh '''
                        cat deploy.yaml
                        sed "s/${IMAGE_TAG}/${BUILD_NUMBER}/g" deploy.yaml
                        git add .
                        ls -lrt
                        git commit -m "Tamjid Ahsan kader"
                        git branch: 'main', credentialsId: 'GitHubCredentials', url: 'https://github.com/JOYMIAH/deployment.git'
                        git push https://github.com/JOYMIAH/deployment.git HEAD:main
                        '''                        
                    }
                }
            }
        }
    }
