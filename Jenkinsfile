// Parameters for the build
properties([
  parameters([
     booleanParam(name: 'runTests', defaultValue: true),
     string(name: 'ARTIFACTORY_LOCAL_RELEASE_REPO'),
     string(name: 'ARTIFACTORY_LOCAL_SNAPSHOT_REPO'),
     string(name: 'ARTIFACTORY_VIRTUAL_RELEASE_REPO'),
     string(name: 'ARTIFACTORY_VIRTUAL_SNAPSHOT_REPO')
  ])
])

node {
  def server
  def rtMaven = Artifactory.newMavenBuild()
  def buildInfo
  environment {
      BUILD_NUMBER = '1.0'
  }
  
  stage('Clone') {
      //clone the project
    git branch: 'master',
        url:    'https://github.com/anuj84/spring-petclinic.git'
  }

  stage ('Artifactory configuration') {
        // Obtain an Artifactory server instance, defined in Jenkins --> Manage Jenkins --> Configure System:
        server = Artifactory.server SERVER_ID

        // Tool name from Jenkins configuration
        rtMaven.tool = MAVEN_TOOL
        rtMaven.deployer releaseRepo: ARTIFACTORY_LOCAL_RELEASE_REPO, snapshotRepo: ARTIFACTORY_LOCAL_SNAPSHOT_REPO, server: server
        rtMaven.resolver releaseRepo: ARTIFACTORY_VIRTUAL_RELEASE_REPO, snapshotRepo: ARTIFACTORY_VIRTUAL_SNAPSHOT_REPO, server: server
        buildInfo = Artifactory.newBuildInfo()
  }
  
 stage ('Exec Maven') {
        rtMaven.run pom: 'maven-examples/maven-example/pom.xml', goals: 'clean install', buildInfo: buildInfo
    }

  stage('Run Tests') {
      // Run Tests if selected
      if (params.runTests){
        sh './mvnw test'
        junit 'target/surefire-reports/*.xml'
    }
  }
  
 stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
  
  stage('Create Image - Dockerfile'){
    sh 'docker build -t petclinic:$BUILD_NUMBER .'  
  }
}
