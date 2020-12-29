pipeline {
agent any
	environment {
		def project_path = "spring-boot-samples/spring-boot-sample-atmosphere"
	}
		stages {
			stage('checkout') {
				steps {
					git 'https://github.com/aasush/jenkins.git'
				}
    		}   		 	 
        	stage('compile,test and package') {
        		steps {
        			dir(project_path) {
            			sh 'mvn clean package'
        			}
            	}
        	} 
        	
    		stage('archival') {
    			steps{
    				dir(project_path) {
            			archiveArtifacts 'target/*.?ar'
    				}
    			}
        	}
			stage('unit test') {
				steps {
					dir(project_path) {
            			junit 'target/surefire-reports/*.xml'
					}
				}
            }
      stage('artifact uploader') {
        steps {
         nexusArtifactUploader artifacts: [[artifactId: 'spring-boot-samples', classifier: '', file: 'spring-boot-sample-atmospher-1.4.0.RELEASE.jar', type: 'jar']], credentialsId: 'nexusid', groupId: 'org.springframework.boot', nexusUrl: '192.168.0.103:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'snapshots', version: '1.4.0.RELEASE'
        }
      }
        }
	post {
		success {
			notify('success')
		}
		failure {
			notify('failed')
		}
		aborted {
			notify('success')
		}
	}
}

def notify(status){
    emailext (
      to: "aasushdisney@gmail.com",
      subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>,
        <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>"""
    )
}
