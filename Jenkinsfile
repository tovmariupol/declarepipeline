pipeline {
    agent {
        label 'Linux_RHEL'
    }
    tools {
        nodejs 'Node12'
    }
    stages {
        stage('Get source') {
            steps {
                git "https://github.com/dbielik/mdt.git"
            }
        }
        stage("Build_CSS") {
            steps {
                sh """cd ${WORKSPACE}/www/css
                  cleancss -d style.css > ../min/custom-min.css"""
            }
        }
        stage("Build_JS") {
            steps {
                sh """cd ${WORKSPACE}/www/js
                  uglifyjs --timings init.js -o ../min/custom-min.js"""
            }
        }
        stage("Package_artifact") {
            steps {
                sh """cd ${WORKSPACE}/www
                  tar --exclude='./css' --exclude='./js' -c -z -f ../site-archive-${params.RELEASE}-${params.RELEASE_VER}-${BUILD_NUMBER}.tgz ."""
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts '*.tgz'
            }
        }
    }
}
