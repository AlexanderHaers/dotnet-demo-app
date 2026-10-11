pipeline {
    agent any
    stages {
        stage('Build and Run') {
            steps {
                sh '''
                # Download docker-compose
                curl -sSL "https://github.com/docker/compose/releases/download/v2.26.1/docker-compose-linux-x86_64" -o docker-compose
                chmod +x docker-compose
                
                ./docker-compose down || true
                docker rm -f todoappdb todoapp || true

                ./docker-compose up -d --build
                '''
            }
        }
        stage('Acceptance Test') {
            steps {
                // Wacht tot de MariaDB en webapp volledig zijn opgestart
                sleep time: 20, unit: 'SECONDS'
                // Test of de webapp bereikbaar is
                sh 'curl http://172.16.0.10:8082/ | grep "Todo"'
            }
        }
    }
}
