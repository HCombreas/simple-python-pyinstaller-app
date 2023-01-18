pipeline {
environment {
registry = "combreas/test1"
registryCredential = 'dockerhub_id'
dockerImage = ''
}
agent any
stages {
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
docker.withRegistry( '', registryCredential ) {
dockerImage.push()
}
}
}
}
stage('run our image') {
steps{
script {
docker.withRegistry( '', registryCredential ) {
dockerImage.run(dockertest)
}
}
}
}
stage('ping our docker') {
steps{
script {
sh('dockertest')
}
}
}


stage('Cleaning up') {
steps{
sh "docker rmi $registry:$BUILD_NUMBER"
}
}
}
}