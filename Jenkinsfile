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
  }
}