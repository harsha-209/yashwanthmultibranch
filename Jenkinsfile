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
          sudo docker tag rummy-server registry-server:8083/rummy-server-${TAG_NAME}:latest
          sudo docker tag rummy-server registry-server:8083/rummy-server-${TAG_NAME}:${BUILD_NUMBER}
        """
      }
    }
    stage('Push to Docker Registry') {
      steps {
        echo 'we will push the image to registry rummy-server-${TAG_NAME}'
        sh """
          sudo docker push registry-server:8083/rummy-server-${TAG_NAME}:latest
          sudo docker push registry-server:8083/rummy-server-${TAG_NAME}:${BUILD_NUMBER}
        """
      }
    }
    stage('Deploy') {
      steps {
        sh """
          sudo ansible-playbook ansible.yml --extra-vars "deployment_host=${INVENTORY_NAME} tag_name=${TAG_NAME}"
        """
      }
    }
  }
}
