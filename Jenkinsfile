// Parameters for the build
properties([
  parameters([
     booleanParam(name: 'runTests', defaultValue: true),
  ])
])

node {
  environment {
      BUILD_NUMBER = '1.0'
  }
  
  stage('Clone') {
      //clone the project
    git branch: BRANCH_NAME,
        url:    'https://github.com/anuj84/spring-petclinic.git'
  }
  
  stage('Build & Install') {
      //build
    sh './mvnw -B -DskipTests clean install'
  }
  
  stage('Run Tests') {
      // Run Tests if selected
      if (params.runTests){
        sh './mvnw test'
        junit 'target/surefire-reports/*.xml'
    }
  }
  
  stage('Package App') {
      //package
    sh './mvnw package'
  }
  
  stage('Create Image - Dockerfile'){
    sh 'docker build -t petclinic:$BUILD_NUMBER .'  
  }
}
