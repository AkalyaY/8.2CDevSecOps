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
           bat 'curl -sSLo sonarscanner.zip https://github.com/SonarSource/sonar-scanner-cli/releases/download/4.6.2.2472/sonar-scanner-cli-4.6.2.2472-linux.zip'
           powershell '''
           Expand-Archive -Path "sonarscanner.zip" -DestinationPath "sonar-scanner"
           '''
           bat './sonar-scanner/bin/sonar-scanner -Dsonar.login=%SONAR_TOKEN%'
         }
       }
     }
  }
}
