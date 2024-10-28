pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

    environment {
        SONARQUBE_SERVER = 'sonarqube'  // Vérifiez que ce nom correspond bien à celui dans la configuration Jenkins
        SONAR_TOKEN = credentials('sonarqubesecret')  // Remplacez par l'ID exact de votre credential
        NEXUS_URL = 'http://192.168.33.10:8081/nexus'
        NEXUS_REPO = 'my-repo'
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
                // Déploiement de l'artefact sur Nexus
                script {
                    def artifactFile = 'target/timesheet-project.jar' // Chemin de l'artefact à déployer, modifiez si nécessaire
                    sh """
                        mvn deploy:deploy-file \
                        -DgroupId=com.example \
                        -DartifactId=timesheet-project \
                        -Dversion=1.0 \
                        -Dpackaging=jar \
                        -Dfile=${artifactFile} \
                        -DrepositoryId=my-repo \
                        -Durl=${NEXUS_URL}/repository/${NEXUS_REPO}/
                    """
                }
            }
    }
}
