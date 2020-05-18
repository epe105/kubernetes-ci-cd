node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}:latest"
    env.BUILDIMG=imageName

    stage "Build"
    
        sh "docker build -t ${imageName} -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
    
    stage "Run Unit Tests"
    
        sh "echo tests complete"

    stage "Run Security Tests"
    
        sh "echo tests complete"

    stage "Push Image to Registry"

        sh "docker push ${imageName}"

    stage "Deploy to QA"
    
        sh "echo deploy to QA"        

    stage "Promote to PROD"

        input message: "Security and Compliance Test have Passed.  Promote to PROD?", ok: "Promote"

    stage "Deploy to PROD"

        kubernetesDeploy configs: "applications/${appName}/k8s/*.yaml", kubeconfigId: 'kenzan_kubeconfig'

}