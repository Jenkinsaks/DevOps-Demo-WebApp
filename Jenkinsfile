pipeline {
  agent any
  stages {
    stage('Message') {
      steps {
        echo 'Jenkins mini pipeline'
      }
    }

    stage('Static Code Analysis') {
      steps {
        withSonarQubeEnv(credentialsId: 'akssonartoken', installationName: 'sonarqube') {
          sh 'mvn -Dsonar.test.exclusions=**/test/java/servlet/createpage_junit.java -Dsonar.login=admin -Dsonar.password=sonar -Dsonar.tests=. -Dsonar.inclusions=**/test/java/servlet/createpage_junit.java -Dsonar.sources=. sonar:sonar -Dsonar.host.url=http://52.152.224.93:9000'
        }

      }
    }

  }
}