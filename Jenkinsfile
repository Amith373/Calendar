pipeline {
    agent any

    environment {
        APP_NAME = "calendar.war"
        SERVER1 = "ec2-user@13.217.222.88"
        SERVER2 = "ubuntu@18.212.203.158"
        TOMCAT_DIR = "/home/ec2-user/tomcat10/webapps"
    }

    stages {
        stage('Build') {
            steps {
                echo "Building WAR from source..."
                sh "mvn clean package -DskipTests"
                sh "cp target/*.war ${APP_NAME}"
            }
        }

        stage('Deploy') {
            steps {
                sshagent(credentials: ['ec2-user']) {
                    echo "Deploying to Server 1..."
                    sh "scp -o StrictHostKeyChecking=no ${APP_NAME} ${SERVER1}:${TOMCAT_DIR}/"

                    echo "Deploying to Server 2..."
                    sh "scp -o StrictHostKeyChecking=no ${APP_NAME} ${SERVER2}:${TOMCAT_DIR}/"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    def urls = [
                        "http://13.217.222.88:8080/calendar/",
                        "http://18.212.203.158:8080/calendar/"
                    ]

                    for (u in urls) {
                        echo "Checking Tomcat server at ${u}..."
                        sh "sleep 10"  // wait for Tomcat to deploy WAR
                        sh "curl -f ${u}"
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

