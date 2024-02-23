pipeline {
  agent none
  stages {
    stage('Build') {
      parallel {
        stage("Build for AMD64 platform") {
          steps {
            script {
              podTemplate(
                name: 'kaniko',
                namespace: 'default',
                // serviceAccount: 'jenkins-sa',
                nodeSelector: 'kubernetes.io/arch=amd64',
                volumes: [
                  // persistentVolumeClaim(mountPath: '/home/jenkins/agent/.m2', claimName: 'jenkins-pv-claim-m2', name: 'm2-home')
                ],
                containers: [
                  containerTemplate(
                    name: 'kaniko',
                    image: 'gcr.io/kaniko-project/executor:debug',
                    command: 'sleep 99d',
                    ttyEnabled: true,
                    volumeMounts: [
                      // volumeMount(mountPath: '/home/jenkins/agent/.m2', name: 'm2-home')
                    ]
                  )
                ],
                securityContext: [
                  fsGroup: 1000
                ],
                restartPolicy: 'Never'
              ) {
                node('kaniko') {
                  container('kaniko') {
                    sh 'echo hello from kaniko'
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