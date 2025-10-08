pipeline {
  agent any

  environment {
    PATH = "C:/Users/Toh Hong Yun/AppData/Local/Programs/Python/Python311;${env.PATH}"
  }

  stages {
    stage('Lint') {
      steps {
        sh """
        pip install ruff mypy
        ruff check .
        mypy .
        """
      }
    }
    stage('Test') {
      steps {
        sh "python3 -m unittest discover ."
      }
    }
    // stage('Deploy') {
    //   when {
    //     expression {
    //       // Only run deploy when this build was triggered by a tag
    //       return env.GIT_BRANCH?.startsWith("origin/tags/") || env.GIT_TAG != null
    //     }
    //   }
    //   steps {
    //     // your deploy commands (see below)
    //   }
    // }
  }
}
