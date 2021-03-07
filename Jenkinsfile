env.DOCKER_REGISTRY = 'vanillavladimir'
env.DOCKER_IMAGE_NAME = 'landing_page'
node('master') {
	stage('HelloWorld') {
      echo 'Hello World'
    }
    stage('Git Pull from Github') {
      git url: 'https://github.com/geraldGhibran/landing-page.git'
    }
    stage('Build Docker Image') {
        sh "docker build --build-arg APP_NAME=landing-page -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."   
    }
    stage('Push Docker Image to Dockerhub') {
        sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage('DeployTo Kubernetes Cluster') {
        sh'''sed -i "39d" landing-page-production.yml'''
        sh'''sed -i "38 a \'\\'        image: vanillavladimir/landing_page:${BUILD_NUMBER}" landing-page-production.yml && sed -i "s/''//" landing-page-production.yml'''
        sh "kubectl create namespace landing-page-production"
        sh "kubectl apply -f landing-page-production.yml"
   }
    stage('Remove Docker Image') {
        sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"   
    }
}





