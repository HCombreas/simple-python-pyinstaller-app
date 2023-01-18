pipeline {
environment {
registry = "combreas/test1"
registryCredential = 'dockerhub_id'
dockerImage = ''
}

agent any
stages {
stage('SCM') {
steps {
git url: 'https://github.com/foo/bar.git'
}
}
stage('build && SonarQube analysis') {
steps {
withSonarQubeEnv('My SonarQube Server') {
// Optionally use a Maven environment you've configured already
withMaven(maven:'Maven 3.5') {
sh 'mvn clean package sonar:sonar'
}
}
}
}
stage("Quality Gate") {
steps {
timeout(time: 1, unit: 'HOURS') {
// Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
// true = set pipeline to UNSTABLE, false = don't
waitForQualityGate abortPipeline: true
}
}
}
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
dockerImage.run()
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
}