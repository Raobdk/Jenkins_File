# Jenkins_File
A Connection of connect with latest updates
pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/yourusername/job-aggregator.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t job-aggregator:latest .'
            }
        }

        stage('Run Scraper') {
            steps {
                sh 'docker run --rm job-aggregator:latest python scraper.py'
            }
        }

        stage('Deploy API') {
            steps {
                script {
                    sh "docker rm -f job-api || true"
                    sh "docker run -d --name job-api -p 5000:5000 job-aggregator:latest"
                }
            }
        }
    }
}



