pipeline {
  agent any
  stages {
    stage('Test') {
      steps {
        sh '''
          docker run --rm \
            -v "$PWD":/work -w /work \
            node:22.20.0-alpine3.22 \
            node --eval "console.log(process.arch, process.platform)"
        '''
      }
    }
  }
}



