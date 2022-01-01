podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: helm-kubectl
        image: dtzar/helm-kubectl
        command:
        - sleep
        args:
        - 99d
''') {
  node(POD_LABEL) {
    stage('Clone docker-registry') {
      git 'https://github.com/AndyG-0/docker-registry.git'
      container('helm-kubectl') {
        stage('Show all pods') {
          sh 'kubectl get pods'
        }
        stage('Show all releases') {
          sh 'helm list'
        }
      }
    }

    stage('Get a Golang project') {
      git url: 'https://github.com/hashicorp/terraform-provider-google.git', branch: 'main'
      container('golang') {
        stage('Build a Go project') {
          sh '''
            mkdir -p /go/src/github.com/hashicorp
            ln -s `pwd` /go/src/github.com/hashicorp/terraform
            cd /go/src/github.com/hashicorp/terraform && make
          '''
        }
      }
    }

  }
}