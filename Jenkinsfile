node('slave') {
  environment{
       credentialsId = "midproject-aws-creds"
  }
    def customImage = ""
    stage("create dockerfile") {
        sh """
            tee Dockerfile <<-'EOF'
            FROM alpine:latest

            RUN apk update && \
                apk add  python3 

            COPY ./requirements.txt /app/requirements.txt

            WORKDIR /app

            RUN pip3 install -r requirements.txt

            COPY . /app

            ENTRYPOINT [ "python3" ]

            CMD [ "app.py" ]
EOF
        """
    }
    stage("build docker") {
        customImage = docker.build("bettyops/opsschool-midproject")
        withDockerRegistry(registry:[
                url: 'https://hub.docker.com/u/bettyops/',            
                credentialsId: 'dockerhub.bettyops'
                ]) {
            customImage.push()
        }
    }
    stage("verify dockers") {
        sh "docker images"
    }
}
