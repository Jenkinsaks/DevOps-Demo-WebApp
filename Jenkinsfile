pipeline {
  agent any

  tools {
        maven = 'Maven'
        jdk = 'jdk'
      }
  
  stages {
    stage('Message') {
      steps {
        echo 'Jenkins mini pipeline'
        //withMaven(jdk: 'jdk', maven: 'Maven', globalMavenSettingsFilePath: '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin', globalMavenSettingsConfig: '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin', mavenLocalRepo: '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin', mavenSettingsConfig: '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin', mavenSettingsFilePath: '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin')
      }
    }

    stage('Static Code Analysis') {
      steps {
        withSonarQubeEnv(credentialsId: 'akssonartoken', installationName: 'sonarqube') {
          sh '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn -Dsonar.test.exclusions=**/test/java/servlet/createpage_junit.java -Dsonar.login=admin -Dsonar.password=sonar -Dsonar.tests=. -Dsonar.inclusions=**/test/java/servlet/createpage_junit.java -Dsonar.sources=. sonar:sonar -Dsonar.host.url=http://52.152.224.93:9000'
        }

      }
    }

    stage('Build Web App') {
      steps {
        sh '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn -B -DskipTests clean compile'
        //sh '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn clean verify'
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
