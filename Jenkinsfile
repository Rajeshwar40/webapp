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
	    
  stage('SAST') {
	  steps {
	  withSonarQubeEnv('devopssecure') {
		  sh 'mvn sonar:sonar'
		  sh 'cat target/sonar/report-task.txt'
	  }
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
	    
	    stage ('DAST') {
		    steps {
			sh 'docker run -t owasp/zap2docker-stable zap-baseline.py -t http://54.245.203.229:8080/WebApp'    
		    }
	    }
	    
    }
}
