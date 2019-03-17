pipeline {
    agent any
    tools { 
        maven 'Maven' 
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }


	stage ('Source-Composition-Analysis') {
	
		steps {
			sh 'rm owasp*'			
sh 'wget https://raw.githubusercontent.com/devopssecure/webapp/master/owasp-dependency-check.sh'
			sh 'chmod +x owasp-dependency-check.sh'			
			sh 'bash owasp-dependency-check.sh'
		}	
	}


        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
      
	stage ('Deploy-To-Tomcat') {
		steps {
			sshagent(['tomcat']) {
				sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.245.203.229:/tmp/apache-tomcat-8.5.38/webapps/'
			}	
		}
	}
    }
}
