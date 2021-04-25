pipeline{
  agent any
  environment{
	imagename = "dikshant1994/php"
    	SERVER_CREDS = credentials('dikshant1994')
  }
  stages{
    stage("Cloning Git"){
      steps{
        echo 'Cloning Repo...'
		git([url: 'https://github.com/dikshantSharma1994/php.git', branch: 'master', credentialsId: 'GITHUB_CREDS'])
      }
    }
    stage('Building image') {
		steps{
			script {
				docker.build imagename
			}
		}
	}
	stage('Deploy Image') {
		steps{
			script {
				docker.withRegistry( '', SERVER_CREDS ) {
				dockerImage.push("$BUILD_NUMBER")
				dockerImage.push('latest')
				}
			}
		}
	}
	stage('Remove Unused docker image') {
		steps{
			sh "docker rmi $imagename:$BUILD_NUMBER"
			sh "docker rmi $imagename:latest"
		}
	}
}
  post {
        success {
            archiveArtifacts artifacts: '**/*.min.*', onlyIfSuccessful: true
        }
    }
}
