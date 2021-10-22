pipeline {
  agent any
  
  tools {
    maven 'Maven'
    nodejs 'NodeJs'
  }

  stages {
    stage('initial'){
      steps{
        sh '''
        echo "PATH = ${PATH}"
        echo "M2_HOME = ${M2_HOME}"
        '''
      }
    }

    stage('Compile'){
      steps{
        sh 'mvn clean compile -e'
      }
    }

    stage('Test'){
      steps{
        sh 'mvn clean test -e'
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