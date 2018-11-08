import hudson.model.*
import hudson.EnvVars
import groovy.json.JsonSlurperClassic
import groovy.json.JsonBuilder
import groovy.json.JsonOutput
import groovy.json.JsonSlurper
import java.net.URL


/* Only keep the 10 most recent builds. */
def projectProperties = [
    [$class: 'BuildDiscarderProperty',strategy: [$class: 'LogRotator', numToKeepStr: '5']],
]
def imageName = 'jenkinsciinfra/jenkinsio'

if (!env.CHANGE_ID) {
    if (env.BRANCH_NAME == null) {
        projectProperties.add(pipelineTriggers([cron('H/30 * * * *'), pollSCM('H/5 * * * *')]))
    }
}

properties(projectProperties)

pipeline {
    agent any
     environment {
    def branch = 'master'
    def pipelineName = 'TestMQ80019021-4860-40fa-82bb-296dafb1703e'
	def file= 'Test_MQ'
    def workspace = pwd()

}
    stages {
			stage('Reemplazando Parametros') {
            steps {
                    script {	
                   echo 'Cambio Parametros'
				   }
        }
		}
        stage('Parametrizando'){
            steps{
                script{
                    def fileJson = workspace+'/'+file+'.json'
             def json = readFile(file:'/var/lib/jenkins/workspace/Test_MQ/Test_MQ.json')
             def data = new JsonSlurperClassic().parseText(json)
             echo " valor:  ${data.pipelineConfig.configuration[12].name}"
             echo " valor:  ${data.pipelineConfig.configuration[12].value}"
             echo " valor:  ${data.pipelineConfig.configuration[12].value[0].key}"
             data.pipelineConfig.configuration[12].value[0].value = "127.0.0.1"
             echo " valor:  ${data.pipelineConfig.configuration[12].value[0].value}"

   
                }
            }
        }
        stage('Despliegue QA') {
            steps {
                    script {	
                   echo 'Desplegando Pipeline --> ' +pipelineName
			 }
            }
        }
		stage('Iniciando QA') {
            steps {
                    script {	
                echo 'Iniciando Pipeline ' 
               			}
            }
        }
		
    }
}








