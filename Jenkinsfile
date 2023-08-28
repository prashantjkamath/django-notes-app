pipeline{
    agent any
    
    stages {
        stage("code") {
            steps {
                echo "clone"
                git url:"https://github.com/prashantjkamath/django-notes-app.git", branch:"main"
            }
        }
        stage("build") {
            steps{
                echo "build"
                sh "docker build -t my-note-app ."
            }
        }
        stage("push") {
            steps {
                echo "push"
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'DOCKER_HUB_PASS', usernameVariable: 'DOCKER_HUB_USER')]) {
                    sh 'echo "$DOCKER_HUB_PASS" | docker login -u "$DOCKER_HUB_USER" --password-stdin'
                    sh "docker tag my-note-app $DOCKER_HUB_USER/my-note-app:latest"
                    sh "docker push $DOCKER_HUB_USER/my-note-app:latest"
                }
            }
        }
        stage("deploy"){
            steps {
                echo "deploy"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
    
    
}
