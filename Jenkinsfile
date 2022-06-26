pipeline {
   agent none
  environment{
       BUILD_SERVER_IP='ec2-user@13.59.2.226'
       IMAGE_NAME='amitv1/java-mvn-privaterepos:php$BUILD_NUMBER'
//This will build the docker with php_Number'
       DEPLOY_SERVER_IP='ec2-user@18.118.103.254'
   }
    stages {          
// BUILDING BLOCK
        stage('BUILD DOCKERIMAGE AND PUSH TO DOCKERHUB') {
            agent any            
            steps {
                script{
                sshagent(['BUILD_SERVER_IP']) {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                echo "Packaging the apps"
                sh "scp -o StrictHostKeyChecking=no -r docker-files ${BUILD_SERVER_IP}:/home/ec2-user"
// docker-files is directory and recursively copying everything to the home directory of ec2 user
                sh "ssh -o StrictHostKeyChecking=no ${BUILD_SERVER_IP} 'bash ~/docker-files/docker-script.sh'"
// Docker script here is :
// sudo yum install docker -y
// sudo systemctl start docker

                sh "ssh ${BUILD_SERVER_KEY} sudo docker build -t ${IMAGE_NAME} /home/ec2-user/docker-files/"
// This image name would give us all DOCKER WITH PHP_NUMBER 
                sh "ssh ${BUILD_SERVER_IP} sudo docker login -u $USERNAME -p $PASSWORD"
                sh "ssh ${BUILD_SERVER_IP} sudo docker push ${IMAGE_NAME}"
              }
            }
            }
        }
        }
       stage('DEPLOY DOCKER CONATINER USING DOCKER_COMPOSE'){
           agent any
           steps{
               script{
                    sshagent(['DEPLOY_SERVER_KEY']){
                         withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                         sh "scp -o StrictHostKeyChecking=no -r docker-files ${DEPLOY_SERVER_IP}:/home/ec2-user"
                         sh "ssh -o StrictHostKeyChecking=no ${DEPLOY_SERVER_IP} 'bash ~/docker-files/docker-script.sh'"
// Docker script here is:
// sudo yum install docker -y
// sudo systemctl start docker

                         sh "ssh ${DEPLOY_SERVER_IP} sudo docker login -u $USERNAME -p $PASSWORD"
                         sh "ssh ${DEPLOY_SERVER_IP} bash /home/ec2-user/docker-files/docker-compose-script.sh ${IMAGE_NAME}"
// Important: from Image_name we are passing variable to shell script
// DOCKER-COMPOSE SCRIPT:
// sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
// sudo chmod +x /usr/local/bin/docker-compose
// sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
// sudo DOCKER_IMAGE=$1 docker-compose -f /home/ec2-user/docker-files/docker-compose.yml up –d

// ***This $1 is positional parameter means first variable we are passing through Jenkins file to yml file.

// And docker-compose.yml file
// version: "3"
// services:
//     web:
//       image: "${DOCKER_IMAGE}"
//       ports:
//       - "8001:80"
//       depends_on:
//       - mysql
//     mysql:
//       image: amitv1/java-mvn-privaterepos:mysql
//       volumes:
//       - db_data:/var/lib/mysql
//       environment:
//         MYSQL_ROOT_PASSWORD: password
//         MYSQL_DATABASE: mydatabase
// volumes:
//    db_data: {}


                         }
                    }
               }
           }
       }
    }
}

