pipeline{
agent any
triggers{
  githubPush()
}
environment{
  APP_NAME = 'sample_app'
  DOCKER_IMAGE = 'thaherathabassum/sample_app'
  BRANCH = 'main'
}
stages{
stage('Checkout'){
steps{
  git branch: "${BRANCH}",
    url: "https://github.com/ThaheraThabassum/devOps-project.git"
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
stage('Initialize'){
  steps{
    script{
      def dockerHome = tool 'myDocker'
      env.PATH = "${dockerHome}/bin:${env.PATH}"
    }
  }
}
stage('Docker Build'){
  steps{
    sh 'docker build -t $DOCKER_IMAGE:ci-$BUILD_NUMBER .'
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
