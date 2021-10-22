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
        	
                sh "${DOCKER_EXEC} rm -f zap2"
        		    sh "${DOCKER_EXEC} pull owasp/zap2docker-stable"
                sh "${DOCKER_EXEC} run --add-host='localhost:192.168.100.4' --rm -e LC_ALL=C.UTF-8 -e LANG=C.UTF-8 --name zap2 -u zap -p 8093:8093 -d owasp/zap2docker-stable zap.sh -daemon -port 8093 -host 0.0.0.0 -config api.disablekey=true"
                sh "${DOCKER_EXEC} --user root -v C:/Users/andre/Desktop/tarea_4/jenkins_home/tools:/zap/wrk/:rw --rm -i owasp/zap2docker-live zap-full-scan.py -t ${TARGET} -f openapi -d -r zap_scan_report.html"
                
                publishHTML([
        				    allowMissing: false,
        				    alwaysLinkToLastBuild: false,
        				    keepAll: false,
        				    reportDir: '/var/jenkins_home/tools/',
        				    reportFiles: 'testreport.html',
        				    reportName: 'HTML Report',
        				    reportTitles: ''])
        		}
            }
        }
  }
}