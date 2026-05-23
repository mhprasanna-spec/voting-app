pipeline {

    agent any

    environment {
        DOCKER_HUB = "prasanna369"
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/mhprasanna-spec/voting-app.git'
            }
        }

        stage('SonarQube Scan') {
            steps {
                sh 'echo sonarqube scan'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_HUB/vote:$BUILD_NUMBER ./vote'
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push $DOCKER_HUB/vote:$BUILD_NUMBER'
            }
        }

        stage('Update Manifest') {
            steps {
                sh '''
                sed -i "s|image:.*|image: $DOCKER_HUB/vote:$BUILD_NUMBER|g" \
                k8s/vote-deployment.yaml
                '''
            }
        }

        stage('Git Push') {
            steps {
                sh '''
                git add .
                git commit -m "updated image"
                git push
                '''
            }
        }
    }
}
