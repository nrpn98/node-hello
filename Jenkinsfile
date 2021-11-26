pipeline {
  agent any
  stages {
    stage("build") {
          steps {
            echo "building the application"
          }
          }
  }
      stages {
        stage("List env vars"){
            steps{
                sh "printenv | sort"
//                 sh "echo ${testvari}"
//                 sh "echo ${testvari1} | cut -c 1-4"
                
            }
        }
    }

}
