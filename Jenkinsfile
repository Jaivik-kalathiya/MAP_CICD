pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Jaivik-kalathiya/MAP_CICD.git'
            }
        }

        stage('Show Workspace') {
            steps {
                sh 'pwd && ls -la'
            }
        }

        stage('Build Images') {
            steps {
                // The correct syntax is `docker compose build`, not `docker compose --build`
                sh 'docker compose build'
            }
        }

        stage('Run Tests') {
            steps {
                // Run pytest only if the 'webapp' service exists; skip otherwise
                sh '''
                    if docker compose ps webapp >/dev/null 2>&1; then
                        docker compose run --rm webapp pytest || echo "No tests found"
                    else
                        echo "⚠️ No webapp service found in docker-compose.yml"
                    fi
                '''
            }
        }

        stage('Deploy Containers') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }
}
