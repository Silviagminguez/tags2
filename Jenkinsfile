// Pipeline para proyectos Android
//
// V.1.0.0
//

def server_up = false

pipeline {
agent any
   // agent { label "sdk5" }
    stages {
        stage('Build') {
            steps {
		bat "./gradlew build"
            }
        }
	stage('Compile') {
	    steps {
            archiveArtifacts artifacts: '**/*.apk', fingerprint: true, onlyIfSuccessful: true            
	    }
	}
	 stage ('sign apk') { 
	    steps{
	    signAndroidApks ( 
	    keyStoreId: "AndroidSign", 
	    keyAlias: "my-alias", 
	    apksToSign: "**/*-unsigned.apk" 
	    ) 
	    }   
	 }
	/* stage('Sonarqube') {
    		environment {
        	scannerHome = tool 'SonarQubeScanner'
    		}
   	    steps {
       		 withSonarQubeEnv('SonarQube') {
           	 bat "${scannerHome}/bin/sonar-scanner -X"
       	    	}
           }
   	 }*/
	stage('Tag') {
            steps {
    		bat "git tag"
    	 
	        script{
	            tag= bat(returnStdout:  true, script: "git describe").trim()
	            echo "La tag es: ${tag}"
	            tag2="${tag}".split("-")[2]
	             echo "La tag2 es: ${tag2}"
	             if (tag2== "QA"){
	                echo "Estoy en QA"
	                }
	             if (tag2== "DES"){
	                echo "Estoy en DES"
	                }
		    if (tag2== "PRO"){
	                echo "Estoy en PRO"
		    }    
		}
            }
        }
    }
}  
