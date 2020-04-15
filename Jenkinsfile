def website_endpoint = "http://adrik-udacity-static.s3-website-eu-west-1.amazonaws.com/"

pipeline {
  agent any
  stages {
    stage('Lint HTML') {
      steps {
        sh 'tidy -q -e *.html'
      }
    }
    stage('Upload to AWS') {
      steps {
        retry(3) {
          withAWS(region:'eu-west-1',credentials:'aws-static') {
            sh 'echo "Uploading content with AWS creds"'
            s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'adrik-udacity-static')
          }
        }
      }
    }
    stage('Verify Website') {
      steps {
        sh 'echo "Verifying website is UP"'
        sh "curl -sSf ${website_endpoint} > /dev/null"
      }
    }
    
  }
}
  
