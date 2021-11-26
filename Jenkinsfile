pipeline {
  agent any
  environment {
    IMAGE_TAG=${GIT_COMMIT}
  }
  stages {
    stage("build") {
          steps {
            echo "building the application"
          }
          }
    stage("List env vars"){
          steps{
            sh "printenv | sort"
            sh "echo ${IMAGE_TAG}"
//            sh "echo ${testvari}"
//                 sh "echo ${testvari1} | cut -c 1-4"
        }
        }
  }
}
