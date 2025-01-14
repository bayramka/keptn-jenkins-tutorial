@Library('keptn-library@3.5')_
def keptn = new sh.keptn.Keptn()

node {
    properties([
        parameters([
         string(defaultValue: 'adidas', description: 'Name of your Keptn Project you have setup for progressive delivery', name: 'Project', trim: false), 
         string(defaultValue: 'staging', description: 'First stage you want to deploy into', name: 'Stage', trim: false), 
         string(defaultValue: 'glass', description: 'Name of the service you provide a configuration change for', name: 'Service', trim: false),
         string(defaultValue: 'docker.io/keptnexamples/carts:0.13.1', description: 'Name of the service you provide a configuration change for', name: 'Image', trim: false),
        ])
    ])

    stage('Initialize Keptn') {

        // Initialize the Keptn Project - ensures the Keptn Project is created with the passed shipyard
        keptn.keptnInit project:"${params.Project}", service:"${params.Service}", stage:"${params.Stage}", keptnConfigureMonitoring:"prometheus" // , shipyard:'shipyard.yaml'

    }

    stage('Trigger Delivery') {
        echo "Progressive Delivery: Triggering Keptn to deliver ${params.Image}"

        // send deployment finished to trigger tests
        def keptnContext = keptn.sendDeliveryTriggeredEvent project:"${params.Project}", service:"${params.Service}", stage:"${params.Stage}", image:"${params.Image}"
        String keptn_bridge = env.KEPTN_BRIDGE
        echo "Open Keptns Bridge: ${keptn_bridge}/trace/${keptnContext}"
    }
    stage('Wait for Result') {
        waitTime = 60

        if(waitTime > 0) {
            echo "Waiting until Keptn is done and returns the results"
            def result = keptn.waitForEvaluationDoneEvent setBuildResult:true, waitTime:waitTime
            echo "${result}"
        } else {
            echo "Not waiting for results. Please check the Keptns bridge for the details!"
        }
    }
}