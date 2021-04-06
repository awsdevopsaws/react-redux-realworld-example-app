pipeline {
  agent {
    docker {
      image 'node:alpine'
      args '-p 3000:3000'
    }
  }
    stages {
        stage('Build') { 
            steps {
                sh '${WORKSPACE}/build.sh ${BRANCH_NAME} ${BUILD_NUMBER}' 
            }
        }
        stage('Publish_artifacts'){
            steps{
                withAWS(region:'eu-central-1', credentials:'tarik-cred') {
                    s3Upload file:"staging/${BRANCH_NAME}_${BUILD_NUMBER}.tar.gzip", bucket:'http://task1-artifacts.s3-website.eu-central-1.amazonaws.com', path:'staging/'
                    s3Upload file:"production/${BRANCH_NAME}_${BUILD_NUMBER}.tar.gzip", bucket:'http://task1-artifacts.s3-website.eu-central-1.amazonaws.com', path:'production/'
                }
            }
        }
        stage('Deploy_Staging'){
            steps{
                withAWS(region:'eu-central-1', credentials:'tarik-cred'){
                    s3Upload bucket:'http://task1-staging.s3-website.eu-central-1.amazonaws.com', includePathPattern:'**/*', workingDir:'staging/build'
                }
            }
        }
    }

  }
