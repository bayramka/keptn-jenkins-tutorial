@Library('keptn-library@3.5')_
def keptn = new sh.keptn.Keptn()

node {
    properties([
        parameters([
         string(defaultValue: 'adidas', description: 'Name of your Keptn Project for Quality Gate Feedback ', name: 'Project', trim: false), 
         string(defaultValue: 'staging', description: 'Stage in your Keptn project used for for Quality Gate Feedback', name: 'Stage', trim: false), 
         string(defaultValue: 'glass', description: 'Servicename used to keep SLIs and SLOs', name: 'Service', trim: false)
        ])
    ])
	
	stage('Checkout SCM') {
	            steps { checkout scm
				}
	}
    stage('Initialize Keptn') {

        // Initialize the Keptn Project - ensures the Keptn Project is created with the passed shipyard
        keptn.keptnInit project:"${params.Project}", service:"${params.Service}", stage:"${params.Stage}", keptnConfigureMonitoring:"prometheus" // , shipyard:'shipyard.yaml'

    }
      
      stage('Show Distribution') {
         steps {
            sh 'cat /etc/issue'
         }
      }
      
      stage('Download Kubectl & Config and install') {
         steps {
            sh 'echo No build required for Webapp.'
            sh 'curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.21.11/bin/linux/amd64/kubectl'
            sh 'chmod +x ./kubectl'
            sh './kubectl version --client'
         }
      }
      
      stage('Download Helm and install') {
         steps {
            sh 'echo No build required for Webapp.'
            sh 'curl -LO https://get.helm.sh/helm-v3.6.0-linux-amd64.tar.gz'
            sh 'tar xvf helm-v3.6.0-linux-amd64.tar.gz'
            sh 'linux-amd64/helm version'
         }
      }
	  
	   /* stage('Build Image') {
         steps {
           sh 'docker image build -t ${REPOSITORY_TAG} .'
         }
      } */
      
      stage('Test Image') {
         steps {
           sh 'echo "Testing..."'
         }
      }
      
      stage('Push Image') {
         steps {
           sh 'echo "Image is pushed to the docker repository."'
         }
      }
      
      stage('Deploy to Dev Environment') {
         steps {
           sh 'echo "Image is pushed to the docker repository."'
         }
      }
      
      stage('Run Load Tests') {
         steps {
           sh 'echo "Image is pushed to the docker repository."'
         }
      }
      
      stage('Deploy To Pre-Prod') {
         steps {
           sh 'echo "Image is pushed to the docker repository."'
         }
      }
      
      stage('Integration Tests') {
         steps {
           sh 'echo "Image is pushed to the docker repository."'
         }
      }
	  
    stage('Trigger Quality Gate') {
        echo "Quality Gates ONLY: Just triggering an SLI/SLO-based evaluation for the passed timeframe"

        // Trigger an evaluation
        def keptnContext = keptn.sendStartEvaluationEvent starttime:"660", endtime:"60"
        String keptn_bridge = env.KEPTN_BRIDGE
        echo "Open Keptns Bridge: ${keptn_bridge}/trace/${keptnContext}"
    }
    stage('Wait for Result') {
        waitTime = 3

        if(waitTime > 0) {
            echo "Waiting until Keptn is done and returns the results"
            def result = keptn.waitForEvaluationDoneEvent setBuildResult:true, waitTime:waitTime
            echo "${result}"
        } else {
            echo "Not waiting for results. Please check the Keptns bridge for the details!"
        }
    }
	
	stage('Release') {
         steps {
           sh 'echo "Image is pushed to the docker repository."'
         }
      }
	  
	stage('Deploy to Production') {
          steps {
            sh './kubectl --kubeconfig=./config apply -f deploy.yaml'
          }
      }
      
      stage('Run Smoke Tests') {
         steps {
           sh 'echo "Image is pushed to the docker repository."'
         }
      }
}