pipeline {
  agent any
  stages {
    stage('Build in Docker') {
      steps {
        sh '''
          docker run --rm \
            -v "$PWD":/workspace -w /workspace \
            -v "$HOME/.m2":/root/.m2 \
            maven:3.9.11-eclipse-temurin-21-alpine \
            mvn -B -DskipTests package
        '''
      }
    }
  }
}

