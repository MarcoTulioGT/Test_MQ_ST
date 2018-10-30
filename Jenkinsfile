import groovy.json.JsonOutput
pipeline {
    agent any
     environment {
    def branch = 'master'
    def pipelineName = 'TestMQ80019021-4860-40fa-82bb-296dafb1703e'
	def file= 'Test_MQ'
       def data = [
        attachments:[
            [
                fallback: "New open task [Urgent]: <http://url_to_task|Test out Slack message attachments>",
                pretext : "New open task [Urgent]: <http://url_to_task|Test out Slack message attachments>",
                color   : "#D00000",
                fields  :[
                    [
                        title: "Notes",
                        value: "This is much easier than I thought it would be.",
                        short: false
                    ]
                ]
            ]
        ]
    ]
}
    stages {
			stage('Reemplazando Parametros') {
            steps {
                    script {	
                   echo 'Cambio Parametros'
				   def props = readJSON file: file+'.json'
				   echo " valor:  ${props.pipelineConfig.title}"
				   echo " valor:  ${props.pipelineConfig.configuration[0].name}"
				   echo " valor:  ${props.pipelineConfig.configuration[12].name}"
				   echo " valor:  ${props.pipelineConfig.configuration[12].value}"
				   def json = JsonOutput.toJson("")
				   echo "json ----> ${data}"
				   props.pipelineConfig.configuration[12].value = data
				   echo "Archivo reemplazado \n ${props}"
				   props.pipelineConfig.title = 'HolaJenkinsFile'
				   writeJSON file: file+'.json', json: props
            }
        }
		}
        stage('Despliegue QA') {
            steps {
                    script {	
                   echo 'Desplegando Pipeline --> ' +pipelineName
				  sh 'curl -X POST --header "Content-Type:application/json" --header "X-Requested-By:SDC" --data @'+file+'.json -u "admin:admin" http://127.1.1.0:18630/rest/v1/pipeline/'+pipelineName+'/import?rev=0&overwrite=true&autoGeneratePipelineId=false&includeLibraryDefinitions=true'
                }
            }
        }
		stage('Iniciando QA') {
            steps {
                    script {	
                echo 'Iniciando Pipeline ' 
                sh 'curl -X POST --header "Content-Type:application/json" --header "X-Requested-By:SDC" --data-binary "{"pMQQueue": "test_mq_queue"}" -u "admin:admin" http://127.1.1.0:18630/rest/v1/pipeline/'+pipelineName+'/start?rev=0'
            				}
            }
        }
		
    }
}








