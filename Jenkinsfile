node{
	
	currentBuild.displayName = "1.${BUILD_NUMBER}"
	def GIT_COMMIT
  stage ('cloning the repository'){
      git 'https://github.com/tapansirol/AltoroJ.git'
  }
	
  stage('Gradle Build') {
       //def gradleHome = tool name : 'mygradle', type 'gradle',
        //sh "${gradleHome}/bin/gradle clean build"
        def path = tool name: 'gradle-4.7', type: 'gradle'
        sh "${path}/bin/gradle build"
   }
   
   stage('SonarQube analysis') {
    def path = tool name: 'gradle-4.7', type: 'gradle'
    withSonarQubeEnv('sonar-server') {
        sh "${path}/bin/gradle --info -Dsonar.host.url=$SONAR_HOST_URL sonarqube"
    }
  }
   //stage ("Appscan"){
   //     appscan application: '84963f4f-0cf4-4262-9afe-3bd7c0ec3942', credentials: 'Credential for ASOC', failBuild: true, failureConditions: [failure_condition(failureType: 'high', threshold: 20)], name: '84963f4f-0cf4-4262-9afe-3bd7c0ec39421562', scanner: static_analyzer(hasOptions: false, target: 'D:/Installables/Jenkins/workspace/Velocity/AltoroJ/build/libs/'), type: 'Static Analyzer'
   //}
	
  stage('Publish Artificats to UCD'){
	  step([$class: 'UCDeployPublisher', component: [componentName: 'AltoroComponent', componentTag: '', delivery: [$class: 'Push', baseDir: 'D:/Installables/Jenkins/workspace/Velocity/AltoroJ/build/libs/', fileExcludePatterns: '', fileIncludePatterns: '*.war', pushDescription: '', pushIncremental: false, pushProperties: 'dockerImage=122285136466.dkr.ecr.us-east-1.amazonaws.com/ci-lab-image:latest', pushVersion: "1.${BUILD_NUMBER}"]], deploy: [createSnapshot: [deployWithSnapshot: true, snapshotName: "1.${BUILD_NUMBER}"], deployApp: 'Altoro', deployDesc: 'Requested from Jenkins', deployEnv: 'Altoro_Dev', deployOnlyChanged: true, deployProc: 'Deploy-Altoro', deployReqProps: '', deployVersions: "1.${BUILD_NUMBER}"], siteName: 'udeploy-server-pipeline'])
 }
}
