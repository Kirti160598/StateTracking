pipeline {
  agent any

  environment {
    GIT_COMMIT_HASH = "${env.GIT_COMMIT ?: sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()}"
    STATE_DIR = "pipeline_state/${GIT_COMMIT_HASH}"
  }

  stages {
    stage('Prepare') {
      steps {
        sh "mkdir -p ${STATE_DIR}"
      }
    }

    stage('Build') {
      when {
        expression { !fileExists("${STATE_DIR}/build.done") }
      }
      steps {
        echo "Running Build Stage..."
        sh 'echo build artifact'
        sh "touch ${STATE_DIR}/build.done"
      }
    }

    stage('Test') {
      when {
        expression { !fileExists("${STATE_DIR}/test.done") }
      }
      steps {
        echo "Running Test Stage..."
        sh 'echo running tests'
        sh "touch ${STATE_DIR}/test.done"
      }
    }

    stage('Deploy') {
      when {
        expression { !fileExists("${STATE_DIR}/deploy.done") }
      }
      steps {
        echo "Running Deploy Stage..."
        sh "touch ${STATE_DIR}/deploy.done"
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: "${STATE_DIR}/*.done", fingerprint: true
    }
  }
}




