pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        build 'build_job'
      }
    }

    stage('test') {
      steps {
        build 'code_quality_job'
      }
    }

    stage('code quality') {
      steps {
        build 'code_quality_job'
      }
    }

    stage('realese') {
      steps {
        build 'release_job'
      }
    }

  }
}