pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh "/opt/apache-maven-3.8.6/bin/mvn package"
            }
        }
        stage('Test') {
            steps {
                sh "/opt/apache-maven-3.8.6/bin/mvn test"
            }
        }
        stage('Release') {
            steps {
              withCredentials([usernamePassword(credentialsId: 'dockerhubcredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh '''
                docker login -u $USERNAME -p $PASSWORD
                docker build -t christianheimke/java-17-demo:${BUILD_NUMBER} ."
                docker push christianheimke/java-17-demo:${BUILD_NUMBER}
                '''
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
              }
            }
        }
    }
}