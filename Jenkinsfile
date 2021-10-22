pipeline {
  agent any
  
  tools {
    maven 'Maven'
    nodejs 'NodeJs'
  }

  stages {
    stage('Scan containers'){
            steps{
                figlet 'Scan Docker'
                script{
                  env.DOCKER = tool "Docker"
                  env.DOCKER_EXEC = "${DOCKER}/bin/docker"
                  sh "${DOCKER_EXEC} run --rm -v C:/Users/andre/Desktop/tarea_4/jenkins_home/workspace/devsecops-sast_feature-jenkins:/root/.cache/ aquasec/trivy openjdk:8-jdk-alpine"
                  sh "${DOCKER_EXEC} rmi aquasec/trivy"
                }
            }
    }
    stage('DAST'){
            steps{
                figlet 'Owasp Zap DAST'
        		
        		script{
        		   env.DOCKER = tool "Docker"
        		   env.DOCKER_EXEC = "${DOCKER}/bin/docker"
        		   env.TARGET = 'http://zero.webappsecurity.com/'
        	
        		    sh "${DOCKER_EXEC} pull owasp/zap2docker-stable"
                sh "${DOCKER_EXEC} run -v C:/Users/andre/Desktop/tarea_4/jenkins_home/workspace/devsecops-sast_feature-jenkins:/zap/wrk/:rw --rm -i owasp/zap2docker-stable zap-full-scan.py -t ${TARGET} -I -r testreport.html -l PASS"
        		}
            }
        }
  }
}