def app = 'Unknown'

pipeline {
    agent any

    stages {
        stage('Build: Dev') {
            steps {
                script{
                    app = docker.build("omega-castle-328311/demo-app","--no-cache -f Dockerfile .")
                    docker.withRegistry('https://us.gcr.io', 'gcr:omega-castle-328311'){
                       app.push("v${env.BUILD_NUMBER}")
                    }
                    sh 'kubectl -n dev set image deployment demo-app demo-app=us.gcr.io/omega-castle-328311/demo-app:v${BUILD_NUMBER}'
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
               sh 'kubectl -n prod set image deployment demo-app demo-app=us.gcr.io/omega-castle-328311/demo-app:v${BUILD_NUMBER}'   
            }
        }
    }
}