def app = 'Unknown'

pipeline {
    agent any

    stages {
        stage('Build: Dev') {
            steps {
                script{
                    app = docker.build("omega-castle-328311/demo-app","--no-cache -f Dockerfile .")
                    docker.withRegistry('https://us.gcr.io', 'gcr:omega-castle-328311'){
                       app.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }
        stage('Approve') {
            steps {
                script{
                    timeout(time: 8, unit: "HOURS") {
                    input message: 'Apporved for Prod', ok: 'Yes'
                    }
                }
            }
        }
    }
}