node('master') {
    // Email recipient
    def emailTo = 'abc@gmail.com'

    // Parameterized cron trigger
    properties([
        pipelineTriggers([
            parameterizedCron('H/5 * * * * %ENABLE_SONAR_ANALYSIS=true')
        ]),
        parameters([
            booleanParam(defaultValue: false, description: 'Enable Sonar Analysis,default is false', name: 'ENABLE_SONAR_ANALYSIS')
        ])
    ])

    try {
        def enableSonarAnalysis = params.ENABLE_SONAR_ANALYSIS

        stage 'SCM Code Checkout'
        echo "Checking out code..."

        stage 'Build'
        echo "Building the project..."

        if(enableSonarAnalysis) {
            stage('SonarQube Analysis') {
                echo "Running SonarQube Analysis..."
            }
        }

        stage 'Complete'
        echo "Build and Analysis (if enabled) complete."

    } catch(exc) {
        currentBuild.result = 'FAILURE'
    } finally {
        if (currentBuild.result == 'FAILURE') {
            echo "Sending failure notification to ${emailTo}"
        } else {
            echo "Sending success notification to ${emailTo}"
        }
    }
}
