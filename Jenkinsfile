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
        sh 'mvn org.owasp:dependency-check-maven:check'
        archiveArtifacts artifacts: 'target/dependency-check-report.html', followSymlinks: false
      }
    }
  }
}