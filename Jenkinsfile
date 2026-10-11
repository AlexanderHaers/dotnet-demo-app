pipeline {
    agent any
    stages {
        stage('Build and Run') {
            steps {
                sh 'docker compose up -d --build'
            }
        }
        stage('Acceptance Test') {
            steps {
                // Wacht tot de MariaDB en webapp volledig zijn opgestart
                sleep time: 20, unit: 'SECONDS'
                // Test of de webapp bereikbaar is
                sh 'curl http://172.16.0.10:8080/ | grep "Todo"'
            }
        }
    }
}
