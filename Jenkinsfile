pipeline {
  agent {
    docker {
      image 'eclipse-temurin:17-jdk-alpine'
    }

  }
  stages {
    stage('build') {
      steps {
        sh './mvnw compile -Dhttps.protocols=TLSv1.2 -Dmaven.repo.local=/tmp/.m2/repository -Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=WARN -Dorg.slf4j.simpleLogger.showDateTime=true -Djava.awt.headless=true --batch-mode --errors --fail-at-end --show-version -DinstallAtEnd=true -DdeployAtEnd=true'
      }
    }

    stage('test') {
      steps {
        sh './mvnw test -Dhttps.protocols=TLSv1.2 -Dmaven.repo.local=/tmp/.m2/repository -Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=WARN -Dorg.slf4j.simpleLogger.showDateTime=true -Djava.awt.headless=true --batch-mode --errors --fail-at-end --show-version -DinstallAtEnd=true -DdeployAtEnd=true'
      }
    }

	stage('build && SonarQube analysis') {
	  steps {
		withSonarQubeEnv('My SonarQube Server') {
		  // Optionally use a Maven environment you've configured already
		  withMaven(maven:'Maven 3.5') {
			sh 'mvn clean package sonar:sonar'
		  }
		}
	  }
	}

	stage("Quality Gate") {
	  steps {
		timeout(time: 1, unit: 'HOURS') {
		  // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
		  // true = set pipeline to UNSTABLE, false = don't
			waitForQualityGate abortPipeline: true
		}
	  }
	}
  }
}