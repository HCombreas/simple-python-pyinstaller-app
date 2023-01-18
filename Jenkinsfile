pipeline {
    environment{
        registry = "combreas/dockerpython"
        registryCredential = 'dockerhub_id'
        dockerImage = ''
        }
agent any
stage('Building our image') {
steps{
script {
dockerImage = docker.build registry + ":$BUILD_NUMBER"
}
}
}
stage('Deploy our image') {
steps{
script {
docker.withRegistry( 'dockerpython', registryCredential ) {
dockerImage.push()
}
}
}
}
stage('Cleaning up') {
steps{
sh "docker rmi $registry:$BUILD_NUMBER"
}
}
}
