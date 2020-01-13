pipeline {
  agent any

  tools {
    maven "apache-maven-3.6.3"
  }
  post {
    always {
        archiveArtifacts artifacts: 'zap-report.html', fingerprint: true
    }
  }

  stages {
    stage('Build') {
      steps {
        git 'https://github.com/ajlanghorn/dvja.git'
        sh "mvn clean package"
      }
    }
    stage('analysis') {
      steps {
        archiveArtifacts artifacts: 'zap-report.html', fingerprint: true
      }
    }
    stage('Publish to S3') {
      steps {
        sh "aws s3 cp /var/lib/jenkins/workspace/dvja/target/dvja-1.0-SNAPSHOT.war s3://ako20-buildartifacts-f1cutinalkuf/dvja-1.0-SNAPSHOT.war"
      }
    }
    stage('Check dependencies') {
      steps {
        dependencyCheck additionalArguments: '', odcInstallation: 'Dependency-Check'
        dependencyCheckPublisher pattern: ''
      }
    }
    stage('Scan for vulnerabilities') {
      steps {
        sh 'java -jar dvja-*.war && zap-cli quick-scan --self-contained --spider -r http://127.0.0.1 && zap-cli report -o zap-report.html -f html'
      }
    }
    stage('Tidy up') {
      steps {
        cleanWs()
      }
    }
  }
}