#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }
    stage('check java') {
        sh "java -version"
    }
    
    stage('clean') {	
        sh "chmod +x mvnw"	
        sh "./mvnw -ntp clean"	
    }	

    stage('install tools') {	
        sh "./mvnw -T 6 -ntp com.github.eirslett:frontend-maven-plugin:install-node-and-npm -DnodeVersion=v10.16.3 -DnpmVersion=6.11.3"	
    }	

    stage('npm install') {	
        sh "./mvnw -T 6 -ntp com.github.eirslett:frontend-maven-plugin:npm"	
    }	

    stage('packaging') {	
        sh "./mvnw -T 4 clean verify -DskipTests"	
        //archiveArtifacts artifacts: '**/target/*.war', fingerprint: true	
    }	

    stage('quality analysis') {
            sh "./mvnw -T 6 sonar:sonar -Dmaven.skip.test=true -Dsonar.host.url=http://localhost:9000/ -Dsonar.login=admin -Dsonar.password=admin"
    }
	
	stage ('deploy to tomcat') {
			sh "mvn tomcat7:redeploy"
	}
	

	
}
