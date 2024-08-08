pipeline{
    agent any
    
    environment{
        SONAR_HOME=tool"sonar"
    }
    stages{
        stage("clone code from Github"){
            steps{
                git url:"https://github.com/Rohan-MK-21/wanderlust.git" , branch:"devops"
            }
        }
        
    stage("sonarqube quality analysis"){
        steps{
            withSonarQubeEnv("sonar"){
                sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=wandelust -Dsonar.projectKey=wandelust"
            }
        }
    }
    stage("owasp Dependency Check"){
        steps{
            dependencyCheck additionalArguments:'--scan ./', odcInstallation: 'dc'
            dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        }
    }
    stage("sonar Quality Gate-scan"){
        steps{
            timeout(time: 2, unit: "MINUTES"){
                waitForQualityGate abortPipeline: false
            }
        }
    }
    stage("Trivy file system scan"){
        steps{
            sh "trivy fs --format table -o trivy-fs-report.html ."
        }
    }
    stage("Deploy using docker compose"){
        steps{
            sh "docker-compose up -d"
        }
    }
    }
}
