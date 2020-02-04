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
        kubernetesDeploy(configs: 'k8s/k8sdeploy.yml', enableConfigSubstitution: true)
    }
    
    stage("expose service") { // Expose the app to the world
        kubernetesDeploy(configs: 'k8s/k8sservice.yml', enableConfigSubstitution: true)
    }
}
