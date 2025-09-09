pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
sh 'wget https://tomcat.apache.org/tomcat-10.0-doc/appdev/sample/sample.war -O sample.war'
            }
        }
        stage('Deploy') {
            steps {
sshagent(['ec2-ssh-key']) {
sh '''
scp -o StrictHostKeyChecking=no sample.war ec2-user@13.217.222.88:/home/ec2-user/tomcat10/webapps/
scp -o StrictHostKeyChecking=no sample.warubuntu@18.212.203.158:/home/ubuntu/tomcat10/webapps/
                    '''
                }
            }
        }  
        stage('Test') {
            steps {
                // Verify Tomcat servers are responding
sh '''
                    echo "Checking Tomcat server 1..."
                    curl -f http://13.217.222.88:8080/sample/ || exit 1

                    echo "Checking Tomcat server 2..."
                    curl -f http://18.212.203.158:8080/sample/ || exit 1
                '''
            }
        }
    }
}
