pipeline {

  agent {
    kubernetes {
      yaml """
kind: Pod
metadata:
  name: node
spec:
  containers:
  - name: node
    image: node:14-alpine
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage('start') {
      steps {
        echo 'hello world'
      }
    }
    stage('writeFile') {
        steps {
            writeFile file: "output/useless3.md", text: "This file is useless"
        }
    }
    stage("archive") {
        steps{
            archiveArtifacts artifacts: 'output/*.md'
        }
    }
    stage("upload"){
        steps {
          rtServer (
    id: 'Artifactory-1',
    url: 'https://ericasrv.jfrog.io/artifactory',
    // If you're using Credentials ID:
    credentialsId: '57f6499f-3c7b-43e4-963c-203ba1fd1083',
    // If Jenkins is configured to use an http proxy, you can bypass the proxy when using this Artifactory server:
    //bypassProxy: true,
    // Configure the connection timeout (in seconds).
    // The default value (if not configured) is 300 seconds:
    timeout: 300
  )
          rtUpload (
              serverId: 'Artifactory-1',
              spec: '''{
                    "files": [
                      {
                        "pattern": "output/*.md",
                        "target": "example-repo-local/"
                      }
                    ]
              }''',
          )
        }
    }
  }
}
