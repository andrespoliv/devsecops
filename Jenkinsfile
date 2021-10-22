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
    stage('Sonarqube'){
      steps{
        script{
          def scannerHome = tool 'SonarQube Scanner'
          def projectKey = "devsecops"
          withSonarQubeEnv('Sonar Server'){
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.java.binaries=target/classes -Dsonar.projectKey=${projectKey} -Dsonar.sources=."
            }
        }
      }
    }
    stage('OWASP Dependency Check'){
      steps{
        sh 'mvn org.owasp:dependency-check-maven:purge'
        sh 'mvn org.owasp:dependency-check-maven:check'
        archiveArtifacts artifacts: 'target/dependency-check-report.html', followSymlinks: false
      }
    }
    stage('Scan containers'){
            steps{
                figlet 'Scan Docker'
                script{
                  env.DOCKER = tool "Docker"
                  env.DOCKER_EXEC = "${DOCKER}/bin/docker"
                  sh "${DOCKER_EXEC} run --rm -v C:/Users/andre/Desktop/tarea_4/jenkins_home/workspace/devsecops-sast_feature-jenkins:/root/.cache/ aquasec/trivy python:3.4-alpine"
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