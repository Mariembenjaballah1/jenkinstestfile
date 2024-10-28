pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

    environment {
        SONARQUBE_SERVER = 'sonarqube'  // Ensure this matches the SonarQube server configuration in Jenkins
        SONAR_TOKEN = credentials('sonarqubesecret')  // Replace with the exact ID of your SonarQube credential
        NEXUS_CREDENTIALS = credentials('nexus_credentials')  // Replace with the correct ID of your Nexus credentials
    }

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
                withSonarQubeEnv('sonarqube') {  // Ensure this matches exactly with Jenkins' SonarQube configuration
                    sh """
                        mvn sonar:sonar \
                        -Dsonar.projectKey=timesheetproject \
                        -Dsonar.host.url=http://192.168.33.10:9000 \
                        -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }

        stage('Deploy to Nexus') {
            steps {
                script {
                    // Deployment to Nexus
                    sh """
                        mvn deploy \
                        -DaltDeploymentRepository=deploymentRepo::default::http://localhost:8081/repository/maven-releases/ \
                        -Dusername=${NEXUS_CREDENTIALS_USR} \
                        -Dpassword=${NEXUS_CREDENTIALS_PSW}
                    """
                }
            }
        }
    }
}
