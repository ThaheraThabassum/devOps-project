pipeline{
agent any
triggers{
  githubPush()
}
environment{
  APP_NAME = 'sample_app'
  DOCKER_IMAGE = 'python-app-image'
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
