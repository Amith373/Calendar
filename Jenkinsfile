pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
sh 'wget https://tomcat.apache.org/tomcat-9.0-doc/appdev/sample/sample.war -O sample.war'
            }
        }
        stage('Deploy') {
            steps {
sshagent(['ec2-ssh-key']) {
sh '''
scp -o StrictHostKeyChecking=no sample.war ec2-user@54.226.130.212:/home/ec2-user/tomcat10/webapps/
scp -o StrictHostKeyChecking=no sample.war ubuntu@98.84.113.241:/opt/tomcat/tomcat9/webapps
                    '''
                }
            }
        }  
        stage('Test') {
            steps {
                // Verify Tomcat servers are responding
sh '''
                    echo "Checking Tomcat server 1..."
                    curl -f http://54.226.130.212:8080/sample/ || exit 1

                    echo "Checking Tomcat server 2..."
                    curl -f http://98.84.113.241:8080/sample/ || exit 1
                '''
            }
        }
    }
}
