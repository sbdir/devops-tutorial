pipeline {
    agent any
    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }
        stage('Build image') {
            steps {
                script {
                    def app = docker.build("edureka1/edureka", ".") // Dockerfileのパスを指定
                    env.APP_IMAGE = app.image // 後続のステージでイメージを参照できるようにする
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    docker.image(env.APP_IMAGE).inside {
                        sh 'echo "Tests passed"' // 実際のテストコマンドに置き換える
                    }
                }
            }
        }
        stage('Push image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image(env.APP_IMAGE).push("${env.BUILD_NUMBER}")
                        docker.image(env.APP_IMAGE).push("latest")
                    }
                }
            }
        }
    }
}