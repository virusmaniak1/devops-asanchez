pipeline {
    agent any

    stages {
        stage('Get code') {
            steps {
                //Obtener codigo del repo
                git 'https://github.com/virusmaniak1/devops-asanchez.git'
            }
        }
        stage('Build'){
            steps {
                echo 'Hacemos el build esto es python no hay que compilar nada'
                echo WORKSPACE
                sh 'ls'
            }
        }
        stage('Tests'){            
            stage('Unit'){
                steps {
                    catchError(buildResult:'UNSTABLE',stageResult:'FAILURE'){
                        echo 'Hacemos las pruebas unitarias'
                        sh '''
                            export FLASK_APP=app/api.py
                            export PATH=$PATH:/opt/homebrew/bin
                            flask run &
                            export PATH=$PATH:/Library/Frameworks/Python.framework/Versions/3.12/bin
                            export PYTHONPATH=.
                            pytest --junitxml=result-unit.xml test/unit
                        '''
                    }
                }
            }
            stage('Rest'){
                steps {
                    catchError(buildResult:'UNSTABLE',stageResult:'FAILURE'){
                        echo 'Hacemos las pruebas unitarias de rest'
                        sh '''
                            export PYTHONPATH=.
                            java -jar /Users/antoniosanchez/Desktop/wiremock-standalone-3.5.4.jar --port 9090 --verbose --root-dir test/wiremock &
                            sleep 10
                            export PATH=$PATH:/Library/Frameworks/Python.framework/Versions/3.12/bin
                            pytest --junitxml=result-rest.xml test/rest
                        '''
                    }
                }
            }            
        }
        
        stage('Results'){
            steps {
                junit 'result*.xml'
                echo 'FINISH'
            }
        }
    }
}