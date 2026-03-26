pipeline {
  agent any

  triggers {
    // 1분마다 소스 코드 변경 사항 확인 (SCM 폴링)
    pollSCM('* * * * *')
  }

  stages {
    stage('Checkout') {
      steps {
        // GitHub 저장소에서 소스 코드 가져오기
        git branch: 'main', url: 'https://github.com/gaejarae/source-maven-java-spring-hello-webapp.git'
      }
    }

    stage('Build') {
      steps {
        // Maven을 사용하여 테스트를 건너뛰고 패키징 (WAR 파일 생성)
        sh 'mvn clean package -DskipTests'
      }
    }

    stage('Test') {
      steps {
        // 단위 테스트 수행
        sh 'mvn test'
      }
    }

    stage('Deploy') {
      steps {
        // 빌드된 WAR 파일을 원격 톰캣 서버에 배포
        deploy adapters: [
          tomcat9(
            credentialsId: 'tomcat-manager',
            url: 'http://192.168.56.102:8080/'
          )
        ], 
        contextPath: 'hello-world', // 이 설정을 통해 /hello-world 경로로 접속 가능해짐
        war: 'target/hello-world.war'
      }
    }
  }
}
