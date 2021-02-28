pipeline {
  agent any
  stages {
    stage('Message') {
      steps {
        echo 'Jenkins mini pipeline'
      }
    }

    stage('Static Code Analysis') {
      agent any
      environment {
        maven = 'Maven'
        jdk = 'jdk'
      }
      steps {
        withSonarQubeEnv(credentialsId: 'akssonartoken', installationName: 'sonarqube') {
          sh '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn -Dsonar.test.exclusions=**/test/java/servlet/createpage_junit.java -Dsonar.login=admin -Dsonar.password=sonar -Dsonar.tests=. -Dsonar.inclusions=**/test/java/servlet/createpage_junit.java -Dsonar.sources=. sonar:sonar -Dsonar.host.url=http://52.152.224.93:9000'
        }

      }
    }

    stage('Build Web App') {
      steps {
        sh '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn -B -DskipTests clean compile'
      }
    }

    stage('Deploy to QA') {
      steps {
        echo 'Deploy to QA'
        deploy(adapters: [tomcat8(credentialsId: 'tomcat-1', path: '', url: 'http://52.255.157.89:8080/')], contextPath: '/QAWebapp', war: '**/*.war')
      }
    }

  }
}