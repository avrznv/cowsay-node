pipeline {
    agent any
    stages {
        stage('Install dependecies') {
            steps {
                dir('./code') {
                sh 'npm install'
                }
            }
        }
    }
}
