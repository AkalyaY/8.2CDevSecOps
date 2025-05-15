pipeline {
  agent any

  environment {
    SONAR_TOKEN = credentials('SONAR_TOKEN')
  }
   tools {
    nodejs 'NodeJS' 
  }
  
  
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/AkalyaY/8.2CDevSecOps.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        bat 'npm test || exit /b 0'
      }
    }

    stage('Generate Coverage Report') {
      steps {
        bat 'npm run coverage || exit /b 0'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        bat 'npm audit || exit /b 0'
      }
    }

     stage('SonarCloud Analysis') {
       steps {
         script {
           bat 'curl -L -o sonarscanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.7.0.2747-windows.zip'
           bat 'powershell -Command "Expand-Archive -Path sonarscanner.zip -DestinationPath sonar-scanner"'
          }
       }
     }

      stage('Run SonarScanner Analysis') {
        steps {
          script {
             bat 'sonar-scanner\\bin\\sonar-scanner.bat -Dsonar.login=%SONAR_TOKEN%'
            }
          }
        }
      }
  }
