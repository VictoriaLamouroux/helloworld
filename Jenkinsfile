pipeline {
     environment {
       IMAGE_NAME = "helloworld"
       IMAGE_TAG = "latest"
       IMAGE_REPO = "victorialamouroux"
     }
     agent none
        stage('Build image') {
            agent any
            steps {
                script {
                  sh 'docker build -t $IMAGE_REPO/$IMAGE_NAME:$IMAGE_TAG .'
                }
            }
        }
        stage('Run container based on builded image') {
            agent any
            steps {
                script {
                  sh '''
                    docker run --name $IMAGE_NAME -d -p 80:80 $IMAGE_REPO/$IMAGE_NAME:$IMAGE_TAG
                    sleep 5
                  '''
                }
            }
        }
        stage('Test image') {
            agent any
            steps {
                script {
                  sh '''
                    curl http://172.17.0.1 | grep -q "Hello universe"
                  '''
                }
            }
        }
        stage('Clean Container') {
            agent any
            steps {
               script {
                 sh '''
                  docker rm -vf ${IMAGE_NAME}
                 '''
                }
            }
        }
    }
}
