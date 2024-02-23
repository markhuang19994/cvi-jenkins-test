def cloud = 'JCR EKS';
pipeline {
  agent none
  stages {
    stage('Build') {
      parallel {
        stage("Build for AMD64 platform") {
          steps {
            script {
              def podYaml = readFile 'Jenkins-kaniko-amd64.yaml'
              podTemplate(
                cloud: cloud,
                yaml: podYaml
                // workspaceVolume: genericEphemeralVolume(accessModes: 'ReadWriteOnce', requestsSize: '10G', storageClassName: 'ebs-sc'),
              ) {
                node(POD_LABEL) {
                  container('kaniko') {
                    sh 'echo hello from kaniko'
                    sh 'sleep 299'
                    sh 'ls -la ~'
                    sh 'ls -la ~/.m2'
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