pipeline {
  agent any

  stages {
    stage('Install dependencies') {
      steps {
        dir('./code') {
            sh 'npm install'
        }
      }
    }
    stage('Build the application and test it') {
        steps {
            dir('./code') {
            sh 'npm start'
            sh 'sleep 2'
            sh 'curl http://localhost:8081/'
            sh 'fuser -k 8081/tcp'
      }
    }
  }
  stage('SonarQube analysis') {
        environment {
                SCANNER_HOME = tool 'SonarQube Scanner-4.8.0.2856'
            }
        steps {
            withSonarQubeEnv('sonarcube-10.0') {
                sh '${SCANNER_HOME}/bin/sonar-scanner \
  -Dsonar.projectKey=Cowsay1 \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.token=sqp_47203bfd988ddf59bc4e5f49e39360683b99b1d7'
            }
        }
    }
  }
}

//Andrey Rozanov
