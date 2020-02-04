node ("ubuntu") {
    def DockerImage = "midproject_app"
    stage("Git") {
        git branch: 'master',
            url: 'https://github.com/Betty42/OpsSchool-Midproject-App.git'
    }
            
    stage("docker build") {
        customImage = docker.build("bettyops/midproject_app")
    }
    
    stage("test") {
        sh 'docker images'
    }
    
    stage("push to Docker Hub") {
        withDockerRegistry(registry:[credentialsId: 'dockerhub']) {
            customImage.push()
        }
    }
    
    stage("deploy") {
        kubernetesDeploy(kubeconfigId: 'e17f6f2f-5636-4ed1-a2e3-180aa99403e9', configs: 'CI-CD/k8sdeploy.yml', enableConfigSubstitution: true)
    }
    
    stage("expose service") { // Expose the app to the world
        kubernetesDeploy(kubeconfigId: 'e17f6f2f-5636-4ed1-a2e3-180aa99403e9', configs: 'CI-CD/k8sservice.yml', enableConfigSubstitution: true)
    }
}
