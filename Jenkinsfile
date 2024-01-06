pipeline {
    agent any 
    stages {
        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Mow Archiving the Artifacts...'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Deploy in Staging Environment') {
            steps {
                build job: 'Deploy_Application_Staging_Env'
            }
        }

        stage('Deploy to Production') {
            steps {
                timeout(time:5, unit: 'DAYS'){
                    input message : 'Approve Production Deployment'
                }


                build job: 'Deploy_Application_Prod_Env'
            }
        }
        
    }
}