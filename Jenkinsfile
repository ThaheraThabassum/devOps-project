pipeline{
agent any
triggers{
  githubPush()
}
environment{
  APP_NAME = 'sample_app'
  DOCKER_IMAGE = 'python-app'
  BRANCH = 'main'
}
stages{
stage('Checkout'){
steps{
  git branch: "${BRANCH}",
    url: "https://github.com/ThaheraThabassum/devOps-project.git",
    credentialsId = 'github-token'
}
}
stage('Build'){
  steps{
    sh 'echo "Build step here"'
}
}
stage('Test'){
  steps{
    sh 'echo "Test step here"'
  }
}
stage('Docker Build'){
  steps{
    sh 'docker build -t $DOCKER_IMAGE:ci-$BUILD_NUMBER .'
  }
}
stage('Docker Push'){
  steps{
    withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'DOCKERHUB_USER', passwordVariable: 'DOCKERHUB_PASS')]){
      sh '''
        echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin
        docker tag $DOCKER_IMAGE:ci-$BUILD_NUMBER $DOCKERHUB_USER/$DOCKER_IMAGE:ci-$BUILD_NUMBER
        docker push $DOCKERHUB_USER/$DOCKER_IMAGE:ci-$BUILD_NUMBER
      '''
    }
  }
}
stage('Update Manifest Repo'){
  steps{
    withCredentials([usernamePassword(credentialsId: 'github-credentials', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]){
    sh '''
      git clone https://${GIT_USER}:${GIT_PASS}@github.com/ThaheraThabassum/devops-project-manifests.git
      cd devops-project-manifests/python-app
      sed -i "s|image: .*|image: thabassumthahera/$DOCKER_IMAGE:ci-$BUILD_NUMBER|g" deployment.yaml
      git config --global user.email "thabassumthahera@gmail.com"
      git config --global user.name "Thahera Thabassum"
      git add deployment.yaml
      git commit -m "Update image tag to thabassumthahera/$DOCKER_IMAGE:ci-$BUILD_NUMBER"
      git push origin main
    '''
  }
}
}
}
post{
  success{
    echo 'CI successful'
  }
  failure{
    echo 'CI failed'
  }
  always{
    echo 'CI completed'
  }
}
}
