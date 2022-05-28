pipeline {
    agent any
    stages {
        stage('Prepare') {
            steps {
                sh 'echo "Prepare Jenkins Step"'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Tests Jenkins Step"'
            }
        }        
        stage('Deploy') {
            steps {
                sh 'echo "Deploy Jenkins Step"'
                sh 'ls -lha'
            }
        }        
    }
}