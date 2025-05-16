pipeline {
    agent {
        node {
            label 'dockerhost-build-server'
        }
    }
    tools {
        maven 'maven-3.9.6'
    }
    stages {
        stage('Packaging') {
            steps {
                echo 'Packaging..'
                sh 'mvn clean package'
            }
        }
        stage('Copying jar file') {
            steps {
                echo 'Copying jar file to build context..'
                sh 'cp target/*.jar .'
            }
        }
        stage('cleanup') {
            steps {
                echo 'Cleaning up old containers and images..'
                sh 'docker system prune -a --volumes --force --filter "label=campaign-demo-server"'
            }
        }
        stage('build image') {
            steps {
                echo 'Building Docker image..'
                sh 'docker build -t nestoralvval/campaign-demo:v1 --label campaign-demo-server .'
            }
        }
        stage('run container') {
            steps {
                echo 'Running Docker container..'
                sh 'docker run -d --name campaign-demo-server --label campaign-demo-server -p 5000:5000 nestoralvval/campaign-demo:v1'
            }
        }
    }
}

