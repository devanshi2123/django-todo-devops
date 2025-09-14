pipeline {
    agent any

    environment {
        SONARQUBE = credentials('sonar-token')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/devanshi2123/django-todo-devops.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                python -m venv venv
                venv\\Scripts\\activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                bat '''
                venv\\Scripts\\activate
                python manage.py test
                '''
            }
        }

        stage('SonarQube Analysis') {
    steps {
        withCredentials([string(credentialsId: 'sonar-token', variable: 'SONARQUBE')]) {
            bat """
            docker run --rm ^
                -e SONAR_HOST_URL="http://host.docker.internal:9000" ^
                -e SONAR_LOGIN="%SONARQUBE%" ^
                -v %cd%:/usr/src ^
                sonarsource/sonar-scanner-cli
            """
        }
    }
}

    }
}
