pipeline {
    agent {
        docker { image 'tcsizmadia/scrapy-jenkins-agent' }
    }
    
    parameters {
        booleanParam(name: 'DRY_RUN', defaultValue: false, description: 'No scraping will happen')
        booleanParam(name: 'SLACK_SEND', defaultValue: false, description: 'Send notification to Slack')
    }
    
    stages {
        stage('Pre-Flight') {
            steps {
                // Check Python version
                bat 'python --version'
                // Check Scrapy version
                bat 'scrapy version'
            }
        }
        stage('Scrape Website') {
            steps {
                script {
                    if (params.DRY_RUN) {
                        echo 'Dry run, no scraping will happen'
                    } else {
                        bat 'scrapy crawl sandbox_spider -o sandbox.json'
                    }
                }
            }
        }
        
        stage('Archive') {
            when {
                expression {
                    currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                // Archive the scraped data
                archiveArtifacts artifacts: 'sandbox.json', fingerprint: true
            }
        }
        
        }
    }
}
