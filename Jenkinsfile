pipeline{
	agent any
	stages{
		stage('Build'){
			steps{
				git branch : 'master', url : 'https://github.com/Agasthyahm/calendar.git'
			}
		}
		stage('Deploy into both server'){
			parallel{
				stage('deploy to server1'){
					steps{
						sh ''' ssh ec2-user@172.31.20.104 "
							scp ec2-user@54.172.218.118:/home/ec2-user/jenkins/workspace/tomcat-deployment/Calendar.war . 
							sudo cp Calendar.war /opt/tomcat/webapps/
							sudo systemctl restart tomcat "
					'''
						
					}
				}
				stage('deploy to server2'){
			
					steps{
						sh ''' ssh ec2-user@172.31.27.245 "
							scp ec2-user@54.172.218.118:/home/ec2-user/jenkins/workspace/tomcat-deployment/Calendar.war .
							sudo cp  Calendar.war /opt/tomcat/webapps/
							sudo systemctl restart tomcat  "
					'''
					}
				}
			}
		}
		stage('Test'){
	
			steps{
				sh ''' 
						echo -e "Testing for server1 \n"
						curl -Is http://3.208.27.235:8080/Calendar/Calendar.html

						echo -e "\n Testing for server2"
						curl -Is http://54.227.57.14:8080/Calendar/Calendar.html
 '''
			}
		}
	}
}

