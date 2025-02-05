pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    echo "docker build"
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh 'docker build -t edgar/dev:latest .'
                    } else if (env.GIT_BRANCH == 'origin/main') {
                        sh 'docker build -t edgar/prod:latest .'
                    }
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    sh 'docker login -u "edgar" -p "$Docker_pass" docker.io'
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh 'docker push edgar/dev:latest'
                    } else if (env.GIT_BRANCH == 'origin/main') {
                        sh 'docker push edgar/prod:latest'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'docker ps -aq | xargs -r docker rm -f'
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh 'docker run -d -p 800:80 edgar/dev:latest'
                    } else if (env.GIT_BRANCH == 'origin/main') {
                        sh 'docker run -d -p 80:80 edgar/prod:latest'
                    }
                }
            }
        }
        stage('Cleanup') {
            steps {
                script {
                    sh 'docker image prune -f'
                }
            }
        }
    }
}
