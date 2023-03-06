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
                    docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
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
                    docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    git branch: 'main', url: 'https://github.com/JOYMIAH/CI-CD-Pipeline-k8s.git'
                        sh '''
                        ls -lrt
                        cat deploy.yml
                        sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deploy.yml
                        cat deploy.yml
                        git add deploy.yml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/JOYMIAH/CI-CD-Pipeline-k8s.git HEAD:main
                        '''                        
                    }
                }
            }
        }
    }
