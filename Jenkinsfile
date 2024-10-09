pipeline {

    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }
environment {
    SONARQUBE_SERVER = 'SonarQube'
    SONAR_TOKEN = credentials('sonaqubesecret') // Replace with the ID of your credential
}

// In the SonarQube Analysis stage:
sh "mvn sonar:sonar -Dsonar.projectKey=timesheetproject -Dsonar.host.url=http://192.168.33.10:9000 -Dsonar.login=${SONAR_TOKEN}"

    stages {
        stage('GIT') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/hwafa/timesheetproject.git'
            }
        }

        stage('Compile Stage') {
            steps {
                sh 'mvn clean compile'
            }
        }
         stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {  // 'SonarQube' doit être le nom configuré dans Jenkins
                  sh "mvn sonar:sonar -Dsonar.projectKey=timesheetproject -Dsonar.host.url=http://192.168.33.10:9000 -Dsonar.login=${SONAR_TOKEN}"
                }
            }
            }
        }
    }

