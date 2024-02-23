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
                annotations: [podAnnotation(key: 'a', value: 'b')], 
                // serviceAccount: 'jenkins-sa',
                nodeSelector: 'kubernetes.io/arch=amd64',
                volumes: [
                  persistentVolumeClaim(mountPath: '/home/jenkins/agent/.m2', claimName: 'maven-m2-pv-claim')
                ],
                // workspaceVolume: genericEphemeralVolume(accessModes: 'ReadWriteOnce', requestsSize: '10G', storageClassName: 'ebs-sc'),
                containers: [
                  containerTemplate(
                    name: 'kaniko',
                    image: 'gcr.io/kaniko-project/executor:debug',
                    command: 'sleep 99d',
                    ttyEnabled: true,
                    runAsUser: '1000',
                    runAsGroup: '1000'
                  )
                ],
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