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


pipeline {
    agent any
    environment {
    // Set the JAVA_HOME environment variable
           JAVA_HOME = '/usr/lib/jvm/default-java'
     }
stages {
stage('Checkout') {
steps {
// Checkout the repository
git 'https://github.com/Aliza-sh/TomcatProject'
}
}
stage('Test') {
steps {
sh 'mvn test'

}
}
stage('Build ') {
steps {
sh 'mvn clean package'

}
}
stage('Package') {
steps {
sh 'mvn package'
}
}

    stage('Deploy to Tomcat') {
        steps {
            sh 'cp target/your-project.war $CATALINA_HOME/webapps/'
            sh '$CATALINA_HOME/bin/catalina.sh run'
        }
    }
}


}