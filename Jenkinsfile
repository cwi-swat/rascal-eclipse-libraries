node {
    try {
        stage('Clone') {
            checkout scm
        }

        withMaven(maven: 'M3', jdk: 'jdk-oracle-8', options: [artifactsPublisher(disabled: true)] ) {
            stage('Build'){
                sh "mvn clean compile"
            }

            stage('Package and Test'){
                sh "mvn package test"
            }

            stage('Deploy'){
                sh "mvn -DskipTests deploy"
            }
            slackSend (color: '#5bc0de', message: "New unstable update available: <${env.BUILD_URL}|${env.JOB_NAME} [${env.BUILD_NUMBER}]>")
        }

    } catch (e) {
        slackSend (color: '#d9534f', message: "FAILED: <${env.BUILD_URL}|${env.JOB_NAME} [${env.BUILD_NUMBER}]>")
            throw e
    }
}   
