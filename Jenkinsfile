 pipeline {
    agent none
    stages {
      stage('Build'){
        parallel {
          stage("Build for AMD64 platform") {
            agent {
                kubernetes {
                  cloud 'JCR EKS'
                  yaml 'Jenkins-kaniko-amd64.yaml'
                }
            }
            steps {
              container('kaniko') {
                sh 'echo hello 123 123'
                //  sh '/kaniko/executor --context `pwd` --dockerfile `pwd`/Dockerfile --destination 899578970796.dkr.ecr.us-west-2.amazonaws.com/java-demo:202310-02-amd64'
              }
            }
          }

          // stage("Build for ARM64 platform") {
          //     agent {
          //        kubernetes {
          //          yamlFile 'Jenkins-kaniko-arm64.yaml'
          //        }
          //     }
          //     steps {
          //       container('kaniko') {
          //          sh '/kaniko/executor --context `pwd` --dockerfile `pwd`/Dockerfile --destination 899578970796.dkr.ecr.us-west-2.amazonaws.com/java-demo:202310-02-arm64'
          //       }
          //     }
          // }
        }
      }

      // stage('Manifest'){
      //   agent {
      //        kubernetes {
      //          yamlFile 'Jenkins-manifest-tool.yaml'
      //        }
      //   }
      //   steps {
      //     container('manifest-tool') {
      //        sh 'docker-credential-ecr-login list'
      //        sh 'chmod 700 ecrtodocker.sh'
      //        sh './ecrtodocker.sh'
      //        sh '/go/src/github.com/manifest-tool/manifest-tool push from-args --platforms linux/amd64,linux/arm64 --template 899578970796.dkr.ecr.us-west-2.amazonaws.com/java-demo:202310-02-ARCHVARIANT --target 899578970796.dkr.ecr.us-west-2.amazonaws.com/java-demo:202310-02'
      //     }
      //   }
      // }
    }
}