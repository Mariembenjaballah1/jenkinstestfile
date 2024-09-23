pipeline {
    agent any

    tools {
        // Use the name defined in Jenkins Global Tool Configuration
         maven "M2_HOME"
    }

    stages {
        stage('MAVEN') {
            steps {
                sh "mvn --version"
            }
        }
    }
}

      
  
