pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'M2_HOME'
    }

    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {

        stage('Commit') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/IhebBM/student-management.git'

                sh '''
                    echo "Dernier commit :"
                    git log -1 --pretty=format:"Hash: %h%nAuteur: %an%nMessage: %s"
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile package -DskipTests'
            }
        }

        stage('Test unitaire') {
            steps {
                sh 'mvn test -Dspring.datasource.url=jdbc:mysql://10.0.2.2:3306/studentdb'
            }

            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            echo 'Pipeline terminé avec succès.'
        }

        failure {
            echo 'Pipeline échoué. Consultez les logs et les résultats des tests.'
        }
    }
}
