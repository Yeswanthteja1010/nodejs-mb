pipeline{
agent any
stages{
stage('clone the repo'){
steps{
checkout scm
}
}

stage('build the image'){
steps{
sh 'docker build -t yeswanthteja/nodemb .'
}
}

stage('push the image'){
steps{
withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'doc_pass', usernameVariable: 'doc_uname')]) {
sh 'docker login -u $doc_uname -p $doc_pass'
sh 'docker push yeswanthteja/nodemb'
}
}
}
stage('deploy'){
when{
branch 'main'
}
steps{
withCredentials([file(credentialsId: 'kubernetes', variable: 'KUBECONFIG')]) {
sh 'kubectl apply -f nodedeployment.yaml'
}
}
}
stage('expose'){
when{
branch 'main'
}
steps{
withCredentials([file(credentialsId: 'kubernetes', variable: 'KUBECONFIG')]) {
sh 'kubectl apply -f nodeservice.yaml'
//echo 'service'
}
}
}
}
}
