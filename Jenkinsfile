pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
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
