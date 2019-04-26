node {
  def mvnHome
  stage('Preparation') { // for display purposes
     // Get some code from a GitHub repository
     git 'https://github.com/yous1235/training-app'
     // Get the Maven tool.
     // ** NOTE: This 'M3' Maven tool must be configured
     // **       in the global configuration.
     mvnHome = tool 'Maven3'
  }
  stage('SonarQube analysis') {
            // Run the maven build
     if (isUnix()) {
        sh "'${mvnHome}/bin/mvn' -B sonar:sonar"
     } else {
        bat(/"${mvnHome}\bin\mvn" -B sonar:sonar/)
     }
       }

  stage('Build') {
     // Run the maven build
     if (isUnix()) {
        sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean build"
     } else {
        bat(/"${mvnHome}\bin\mvn" -B -DskipTests -Dmaven.test.failure.ignore clean package/)
     }
  }

  stage('Test') {
     if (isUnix()) {
        sh "'${mvnHome}/bin/mvn' test"
     } else {
        bat(/"${mvnHome}\bin\mvn" test/)
     }
  }

  stage('Results') {
     junit '**/target/surefire-reports/TEST-*.xml'
     archiveArtifacts  'target/*.jar'
  }

     stage('Publish Artefact') {

     if (isUnix()) {
        sh "'${mvnHome}/bin/mvn' -DdeployOnly deploy"
     } else {
        bat(/"${mvnHome}\bin\mvn" -Dmaven.test.skip=true deploy -s "C:\apache-maven-3.6.0\conf\settings.xml"/)
     }
  }

  stage('Execute Jar') {
     if (isUnix()) {
        sh "java -jar  target/*.jar"
     } else {
        bat(/java -jar target\/training-app-1.1-SNAPSHOT-jar-with-dependencies.jar/)
     }
  }
}
