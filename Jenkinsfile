def docker_images = ["cd2team/docker-python:2.7", "cd2team/docker-python:3.5", "cd2team/docker-python:3.6", "cd2team/docker-python:3.7"]

def get_stages(docker_image) {
    stages = {
        docker.image(docker_image).inside {
            stage("${docker_image}") {
                echo "Running in ${docker_image}"
                echo "${UID} ${USER} ${PWD}"
            }
            stage('build') {
                sh "pip install -r requirements.txt --user"
            }
            stage('test') {
                sh "pytest"
            }

        }
    }
    return stages
}

node('master') {
    def stages = [:]
    
    for (int i = 0; i < docker_images.size(); i++) {
        def docker_image = docker_images[i]
        stages[docker_image] = get_stages(docker_image)
    }
    
    stage('checkout') {
        checkout scm
    }
    parallel stages
}
