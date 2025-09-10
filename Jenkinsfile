pipeline{
	agent none
	stages{
		stage('Build'){
		agent { label 'node2' }
			steps{
				git branch : 'master', url : 'https://github.com/Agasthyahm/calendar.git'
			}
		}
		stage('Deploy into both server'){
			parallel{
				stage('deploy to server1'){
				agent { label 'node2' }
					steps{
						sh ''' ssh ec2-user@172.31.41.188 "
							scp ec2-user@172.31.47.51:/home/ec2-user/jenkins/workspace/tomcat-deployment/Calendar.war . 
							sudo cp Calendar.war /opt/tomcat/webapps/
							sudo systemctl restart tomcat "
					'''
						
					}
				}
				stage('deploy to server2'){
				agent { label 'node2' }
					steps{
						sh ''' ssh ec2-user@172.31.36.199 "
							scp ec2-user@172.31.47.51:/home/ec2-user/jenkins/workspace/tomcat-deployment/Calendar.war .
							sudo cp  Calendar.war /opt/tomcat/webapps/
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
						curl -Is http://51.20.4.202:8080/Calendar/Calendar.html

						echo -e "\n Testing for server2"
						curl -Is http://16.170.206.50:8080/Calendar/Calendar.html
 '''
			}
		}
	}
}

