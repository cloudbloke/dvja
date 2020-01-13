pipeline {
  agent any

  tools {
    maven "apache-maven-3.6.3"
  }

  stages {
    stage('Build') {
      steps {
        git 'https://github.com/ajlanghorn/dvja.git'
        sh "mvn clean package"
      }
    }
    stage('Publish to S3') {
      steps {
        sh "aws s3 cp /var/lib/jenkins/workspace/dvja/target/dvja-1.0-SNAPSHOT.war s3://CHANGE-ME/dvja-1.0-SNAPSHOT.war"
      }
    }
    stage('Check dependencies') {
      steps {
        dependencyCheck additionalArguments: '', odcInstallation: 'Dependency-Check'
      }
    }
    stage('Tidy up') {
      steps {
        cleanWs()
      }
    }
  }
}

stage('Check dependencies') {
steps {
dependencyCheck additionalArguments: '', odcInstallation: 'Dependency-Check'
}
}