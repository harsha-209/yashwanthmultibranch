pipeline {
  agent any
  environment {
    PRE_PROD_BRANCH = "integration"
    STAGING_BRANCH = "main"
    BUILD_ENV = """${	
      BRANCH_NAME ==~ PRE_PROD_BRANCH ? 'Dev' : 	
      BRANCH_NAME == STAGING_BRANCH ? 'QA' : 'Dev'	
    }"""
  }
  stages {
    stage('Create Docker Image') {
      steps {
        echo "The branch: ${BRANCH_NAME} and the build ${BUILD_NUMBER}"
        sh """
          sudo docker build --build-arg BUILD_ENV=${BUILD_ENV}  -t rummy-server .
        """
      }
    }
    stage('Push to Docker Registry') {
      steps {
        echo 'we will push the image '
        sh """
          echo "getting"
        """
      }
    }
    stage('Deploy') {
      steps {
        sh """
          echo "ok"
        """
      }
    }
  }
}
