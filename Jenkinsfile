pipeline {
  agent any

  environment {
    // Run Gradle inside a container, mounting the workspace
    GRADLE_IMG = 'gradle:8.8-jdk21-alpine'
    RUN = 'docker run --rm -u "$(id -u):$(id -g)" -v "$PWD":/workspace -w /workspace ${GRADLE_IMG} sh -lc'
  }

  stages {
    stage('Bootstrap project (if missing)') {
      steps {
        sh '''
          set -eu
          # Create minimal Gradle Java project only if it doesn't exist
          if [ ! -f settings.gradle ]; then
            cat > settings.gradle <<'EOF'
rootProject.name = "hello"
EOF
          fi

          if [ ! -f build.gradle ]; then
            cat > build.gradle <<'EOF'
plugins { id 'java' }
repositories { mavenCentral() }
dependencies { testImplementation 'org.junit.jupiter:junit-jupiter:5.10.2' }
test { useJUnitPlatform() }
EOF
          fi

          mkdir -p src/test/java/example
          if [ ! -f src/test/java/example/AppTest.java ]; then
            cat > src/test/java/example/AppTest.java <<'EOF'
package example;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
class AppTest {
  @Test void adds() { assertEquals(2, 1 + 1); }
}
EOF
          fi
        '''
      }
    }

    stage('Create wrapper') {
      steps {
        sh '''
          set -eu
          ${RUN} 'gradle --version && gradle wrapper --gradle-version 8.8'
          chmod +x gradlew
        '''
      }
    }

    stage('Test') {
      steps {
        sh '''
          set -eu
          ${RUN} './gradlew --no-daemon check'
        '''
      }
    }
  }

  post {
    always {
      junit 'build/test-results/**/*.xml'
      archiveArtifacts artifacts: 'build/reports/tests/test/**/*', allowEmptyArchive: true
    }
  }
}




