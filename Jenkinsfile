pipeline {
    agent {label "agent-local"}

    stages {
        stage('Code Clone') {
            steps {
                git url: 'https://github.com/imranyousaf282/two-tier-flask-app.git/',branch:"master"
            }
        }
        stage('Build') {
            steps {
                echo 'This is building the code'
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage('Push on docke hub') {
            steps {
                echo 'This is pushing the code on server'
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCred",
                    passwordVariable:"dockerHubPass",
                    usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app:latest"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }
        stage('Deploy the code on server') {
            steps {
                echo 'This is deploy the code on server'
                sh "docker compose up -d"
            }
        }
    }
    post {
        success {
            emailext(
                from: 'imranyousaf8282@gmail.com',
                to: 'imranyousaf8282@gmail.com',
                subject: 'Congratulations! Build success of two-tier-flask-app',
                body: 'Congratulations! Build success of two-tier-flask-app'
                )
                }
        failure {
            emailext(
                from: 'imranyousaf8282@gmail.com',
                to: 'imranyousaf8282@gmail.com',
                subject: 'Build failed: two-tier-flask-app',
                body: 'Build failed: two-tier-flask-app'
                )
                }
        }
}


