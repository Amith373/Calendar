pipeline {
    agent any

    environment {
        APP_NAME = "calendar.war"
        SERVER1 = "ec2-user@<13.217.222.88>"
        SERVER2 = "ec2-user@<18.212.203.158>"
        TOMCAT_DIR = "/home/ec2-user/tomcat/webapps"
    }

     stage('Build') {
       steps {
             echo "Cloning repository and preparing WAR..."
            git branch: 'master', url: 'https://github.com/Amith373/Calendar.git'
         }
      }

        stage('Deploy') {
            steps {
                sshagent(credentials: ['ec2-user']) {
                    echo "Deploying to Server 1..."
                    sh "scp -o StrictHostKeyChecking=no Assignment 18.212.203.158:/home/ec2-user/tomcat10/webapps/"

                    echo "Deploying to Server 2..."
                    sh "scp -o StrictHostKeyChecking=no Assignment 18.212.203.158:/home/ubuntu/tomcat10/webapps/"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    def urls = [
                        "http://<18.212.203.158P>:8080/calendar/",
                        "http://<18.212.203.158>:8080/calendar/"
                    ]

                    for (u in urls) {
                        echo "Testing deployment at ${u}"
                        sh "curl -s --head ${u} | grep '200 OK'"
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful on both servers!"
        }
        failure {
            echo "❌ Deployment failed. Please check logs."
        }
    }
}

