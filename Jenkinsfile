def cloud = 'JCR EKS';
pipeline {
  agent none
  stages {
    stage('Build') {
      parallel {
        stage("Build for AMD64 platform") {
          steps {
            script {
              podTemplate(
                cloud: cloud,
                name: 'kaniko',
                namespace: 'default',
                podAnnotation(key: 'a', value: 'b')], 
                labels: [
                  {
                    key: 'c-d-e-f-g',
                    value: 'd'
                  }
                ],
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
              ) {
                node(POD_LABEL) {
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