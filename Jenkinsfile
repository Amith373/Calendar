pipeline{
	agent any
	stages{
		stage('Build'){
		agent { label 'node2' }
			steps{
				git branch : 'master', url : 'https://github.com/Amith373/Calendar.git'
			}
		}
		stage('Deploy into both server'){
			parallel{
				stage('deploy to server1'){
				agent { label 'node2' }
					steps{
						sh ''' ssh ec2-user@172.31.20.104 "
							scp ec2-user@54.88.24.75:/home/ec2-user/jenkins/workspace/tomcat-deployment/Calendar.war . 
							sudo cp Calendar.war /home/ec2-user/tomcat10/webapps/
							sudo systemctl restart tomcat "
					'''
						
					}
				}
				stage('deploy to server2'){
				agent { label 'node2' }
					steps{
						sh ''' ssh ec2-user@172.31.27.245 "
							scp ec2-user@54.88.24.75:/home/ec2-user/jenkins/workspace/tomcat-deployment/Calendar.war .
							sudo cp  Calendar.war /home/ubuntu/tomcat10/webapps
							sudo systemctl restart tomcat  "
					'''
					}
				}
			}
		}
		stage('Test'){
		agent { label 'node2' }
			steps{
				sh ''' 
						echo -e "Testing for server1 \n"
						curl -Is http://13.217.222.88:8080/Calendar/Calendar.html

						echo -e "\n Testing for server2"
						curl -Is http://18.212.203.158:8080/Calendar/Calendar.html
 '''
			}
		}
	}
}
