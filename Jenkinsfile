pipeline {
    agent any

    environment {
        DOTNET_VERSION = '6.0.x'
    }

    triggers {
        // По-добре е да използваш webhook, но ако държиш на polling:
        pollSCM('* * * * *')
    }

    // Ограничаваме изпълнението само за клоните main и feature*
    when {
        branch pattern "main|feature.*"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Извличане на кода от ${env.BRANCH_NAME} клона..."
                checkout scm
            }
        }

        stage('Setup .NET') {
            steps {
                echo "Проверка на .NET SDK версия"
                sh 'dotnet --version'
            }
        }

        stage('Restore dependencies') {
            steps {
                echo 'Възстановяване на зависимости...'
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo 'Билд на приложението...'
                sh 'dotnet build --no-restore'
            }
        }

        stage('Test') {
            steps {
                echo 'Изпълнение на всички тестове...'
                sh 'dotnet test --no-build --verbosity normal'
            }
        }
    }
}
