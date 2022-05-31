pipeline {
    agent any
    stages {
        stage('CI pipeline') {
            steps {
                sh 'cat ci_pipeline.txt'
                sh 'echo "Build Step"'
                sh 'echo "Security Step"'
                sh 'echo "Test Step"'
                sh 'echo "QA Step"'
            }
        }     
        stage('CD pipeline') {
            steps {
                sh 'cat cd_pipeline.txt'
                sh 'echo "Release Step"'
                sh 'echo "Deploy Step"'
            }
        }        
    }
}