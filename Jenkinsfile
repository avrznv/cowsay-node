pipeline {
  agent any

  stages {
    stage('Install dependencies using npm') {
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
            sh 'sleep 1'
        }
        script {
          try {
            // Execute curl command to check if the server is accessible
            sh 'curl http://localhost:8081/'
          } catch (Exception e) {
            // If curl command fails, mark the build as failed
            error('Server not accessible')
          }
          try {
            slackSend(
              channel: '#jenkins-notification-katya',
              color: 'good',
              message: "The cowsay is build, hurray!!!",
              token: 'devopszionetm-slack'
            )
          } catch (Exception e) {
            echo "Error sending the Slack notification ${e.message}"
          }
        sh 'fuser -k 8081/tcp'
      }
    }
  }
  stage('SonarQube analysis') {
        environment {
                SCANNER_HOME = tool 'SonarQubeScanner-4.8.0'
            }
        steps {
            withSonarQubeEnv('sonarcube-cowsay') {
                sh '${SCANNER_HOME}/bin/sonar-scanner \
  -Dsonar.projectKey=cowsay-node2 \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.token=sqp_3205865dfdd2ef5f13cf155ee9bca2aa8b84ce22'
            }
        }
    }
}
}
