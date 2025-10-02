pipeline {
  agent any
  stages {
    stage('Build in Docker') {
      steps {
        sh '''
          docker run --rm -v "$PWD":/workspace -w /workspace alpine \
            sh -c 'echo Pipeline is working! && ls -la'
        '''
      }
    }
  }
}


