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
                    sh 'kubectl set image deployment demo-app demo-app=us.gcr.io/omega-castle-328311/demo-app:${BUILD_NUMBER}'
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
        stage('Deploy: Prod') {
            steps {
               sh 'kubectl set image -n prod deployment demo-app demo-app=us.gcr.io/omega-castle-328311/demo-app:${BUILD_NUMBER}'   
            }
        }
    }
}