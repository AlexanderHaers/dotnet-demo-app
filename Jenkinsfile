pipeline {
    agent any
    stages {
        stage('Unit Tests') {
            steps {
                sh '''
                # 1. Verwijder eventuele oude test-containers
                docker rm -f test-runner || true
                
                # 2. Start een tijdelijke container op de achtergrond (.NET 10)
                docker run -d --name test-runner -w /app mcr.microsoft.com/dotnet/sdk:10.0 sleep 600
                
                # 3. Kopieer de Jenkins workspace bestanden direct de container in (dit omzeilt de bug!)
                docker cp . test-runner:/app/
                
                # 4. Voer de tests uit
                docker exec test-runner dotnet test
                
                # 5. Ruim de test-container op
                docker rm -f test-runner
                '''
            }
        }
        stage('Build and Run') {
            steps {
                # Let op: zie waarschuwing hieronder over 'docker compose'
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
