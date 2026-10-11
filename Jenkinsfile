pipeline {
    agent any
    stages {
        stage('Build and Run') {
            steps {
                sh '''
                # Download de officiële docker-compose tool lokaal in de workspace
                curl -sSL "https://github.com/docker/compose/releases/download/v2.26.1/docker-compose-linux-x86_64" -o docker-compose
                chmod +x docker-compose
                
                # Start de MariaDB en webapp met de zojuist gedownloade tool
                ./docker-compose up -d --build
                '''
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
