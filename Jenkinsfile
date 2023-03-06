pipeline {
    
    agent any 
    
    environment {
        DOCKERHUB_USERNAME = "joymiah1"
        IMAGE_TAG = "${BUILD_NUMBER}"
        APP_NAME= "todo_app"
        IMAGE_NAME= "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"

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
                    docker login -u joymiah1 -p Joy--4108
                    echo 'Push to Dockerhub Repositiory'
                    docker push joymiah1/todo_app:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git branch: 'main', url: 'https://github.com/JOYMIAH/Deployment_ManifestFile.git'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    git branch: 'main', credentialsId: 'githubtoken1', url: 'https://github.com/JOYMIAH/Deployment_ManifestFile.git' 
                        sh '''
                        cat deploy.yaml
                        sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/JOYMIAH/Deployment_ManifestFile.git HEAD:main
                        '''                        
                    }
                }
            }
        }
    }
