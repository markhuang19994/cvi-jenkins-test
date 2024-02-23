def cloud = 'JCR EKS';
pipeline {
  agent {
    label 'master || built-in'
  }

  stages {
    stage('Build') {
      parallel {
        stage("Build for AMD64 platform") {
          steps {
            script {
              def podYml = readFile 'Jenkins-kaniko-amd64.yaml'
              podTemplate(
                cloud: cloud,
                yaml: podYml,
                // serviceAccount: 'jenkins-sa',
                nodeSelector: 'kubernetes.io/arch=amd64',
                volumes: [
                  persistentVolumeClaim(mountPath: '/home/jenkins/agent/.m2', claimName: 'maven-m2-pv-claim')
                ],
                workspaceVolume: genericEphemerdynamicPVCalVolume(accessModes: 'ReadWriteOnce', requestsSize: '10G', storageClassName: 'ebs-sc'),
              ) {
                node(POD_LABEL) {
                  container('kaniko') {
                    sh 'echo hello from kaniko'
                    sh '`pwd`/Dockerfile'
                    // sh '/kaniko/executor --context `pwd` --dockerfile `pwd`/Dockerfile --destination your-destination'
                  }
                }
              }
            }
          }
        }
      }
    }
  }
}