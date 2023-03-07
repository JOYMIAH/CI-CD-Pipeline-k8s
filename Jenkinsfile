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
                    docker build -t :${BUILD_NUMBER} .
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
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/JOYMIAH/deployment.git'
                        sh '''
                        cat deploy.yaml
                        sed -i '' "s/32/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/JOYMIAH/deployment.git HEAD:main
                        '''                        
                    }
                }
            }
        }
    }
