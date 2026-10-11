pipeline {
    agent any
    stages {
        stage('Unit Tests') {
            steps {
                // This downloads a temporary .NET 10 container, maps your code into it, and runs the tests
                sh 'docker run --rm -v ${WORKSPACE}:/app -w /app mcr.microsoft.com/dotnet/sdk:10.0 dotnet test'
            }
        }
        stage('Build and Run') {
            steps {
                // This uses the docker-compose.yml file to build the webapp and start the MariaDB database
                sh 'docker compose up -d --build'
            }
        }
        stage('Acceptance Test') {
            steps {
                // Wait for the database and web app to fully initialize
                sleep time: 20, unit: 'SECONDS'

                // Test if the web app is running successfully on the Docker host IP
                sh 'curl http://172.16.0.10:8080/ | grep "Todo"'
            }
        }
    }
}
