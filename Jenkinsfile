pipeline {
    agent {
      label "jenkins-node"
    }

  triggers {
    pollSCM('* * * * *')
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/gaejarae/source-maven-java-spring-hello-webapp.git'
      }
    }
    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }
    stage('Deploy') {
      steps {
        deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tomcat-manager', path: '', url: 'http://192.168.56.102:8080/')], contextPath: null, war: 'target/hello-world.war'
      }
    }
  }
}
