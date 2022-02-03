pipeline {
  agent any
  environment {
    PRE_PROD_BRANCH = /^release\/.*/
    STAGING_BRANCH = "integration"
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
        echo 'we will push the image to registry rummy-server-${TAG_NAME}'
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
