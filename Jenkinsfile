pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Configure') {
            steps {
                sh 'chmod +x ./jenkins/scripts/test.sh'
                sh 'chmod +x ./jenkins/scripts/deliver.sh'
                sh 'chmod +x ./jenkins/scripts/kill.sh'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
                withSonarQubeEnv('SonarQube') {
                    sh "npx sonar-scanner -D sonar.projectKey=React-SonarQube -D sonar.login=b082a592b5caae1791279ed841c00f3d865a24e4"
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
