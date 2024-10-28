pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

    environment {
        SONARQUBE_SERVER = 'sonarqube'  // Vérifiez que ce nom correspond bien à celui dans la configuration Jenkins
        SONAR_TOKEN = credentials('sonarqubesecret')  // Remplacez par l'ID exact de votre credential
        NEXUS_CREDENTIALS = credentials('nexus_credentials') 
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
                withSonarQubeEnv('sonarqube') {  // Assurez-vous que ce nom correspond exactement
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
                    // Déploiement vers Nexus
                    sh "mvn deploy -DaltDeploymentRepository=deploymentRepo::default::http://localhost:8081/repository/maven-releases/ -Dusername=${NEXUS_CREDENTIALS.username} -Dpassword=${NEXUS_CREDENTIALS.password}"
                }
            }
    }
}
}
