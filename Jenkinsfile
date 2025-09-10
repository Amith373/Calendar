pipeline{
	agent none
	stages{
		stage('Build'){
		agent { label 'master' }
			steps{
				git branch : 'master', url : 'https://github.com/Amith373/Calendar.git'
			}
		}
		stage('Deploy into both server'){
			parallel{
				stage('deploy to server1'){
				agent { label 'master' }
					steps{
						sh ''' ssh ec2-user@172.31.20.104 "
							scp ubuntu@54.81.207.39:/home/ec2-user/jenkins/workspace/tomcat-deployment/Calendar.war . 
							sudo cp Calendar.war /opt/tomcat/webapps/
							sudo systemctl restart tomcat "
					'''
						
					}
				}
				stage('deploy to server2'){
				agent { label 'master' }
					steps{
						sh ''' ssh ubuntu@172.31.27.245 "
							scp ubuntu@54.81.207.39:/home/ec2-user/jenkins/workspace/tomcat-deployment/Calendar.war .
							sudo cp  Calendar.war /opt/tomcat/webapps/
							sudo systemctl restart tomcat  "
					'''
					}
				}
			}
		}
		stage('Test'){
		agent { label 'master' }
			steps{
				sh ''' 
						echo -e "Testing for server1 \n"
						curl -Is http://18.234.215.210:8080/Calendar/Calendar.html

						echo -e "\n Testing for server2"
						curl -Is http://18.212.248.90:8080/Calendar/Calendar.html
 '''
			}
		}
	}
}
