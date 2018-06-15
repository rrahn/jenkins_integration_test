import jenkins.branch.BranchEventCause;
import org.jenkinsci.plugins.workflow.cps.replay.ReplayCause;

pipeline {
    agent any
    environment
    {
        PR_NUMBER = "${GITHUB_PR_NUMBER}"
        BUILD_TRIGGER="none"   // [PR, CRON, TAG, USER]
    }
    stages
    {
        stage ('Configure')
        {
            steps
            {
                echo "My PR ${env.PR_NUMBER}"

            }
        }
        stage ('Prepare')
        {
            steps
            {
                script
                {
                    for (cause in currentBuild.rawBuild.getCauses())
                    {
                        if (cause instanceof BranchEventCause)
                        {
                            echo "Triggered by BranchEventCause!"
                            env.BUILD_TRIGGER = "PR"
                        }
                        else if (cause instanceof ReplayCause)
                        {
                            echo "Triggered by ReplayCause!"
                        }
                        // else if (cause instanceof triggers.TimerTrigger.TimerTriggerCause)
                        // {
                        //     echo "Triggered by cron."
                        //     env.BUILD_TRIGGER = "CRON"
                        // }
                        // else if ()
                        // {
                        //
                        // }
                        echo "to string ${cause.toString()}"
                    }
                }
            }
        }
    //     stage ('Parallel') {
    //         parallel {
    //             stage('Build and tun unit tests on linux') {
    //                 agent none
    //                 steps {
    //                     echo "This PR ${PR_NUMBER}"
    //                     echo "Build trigger: ${BUILD_TRIGGER}"
    //                     //build job: 'pipeline_unit_tests_unix', parameters: [string(name: 'GITHUB_PR_NUMBER', value: "${GITHUB_PR_NUMBER}")]
    //
    //                     /*
    //                     echo "Workspace: ${WORKSPACE}"
    //                     copyArtifacts filter: "build-${PR_NUMBER}", fingerprintArtifacts: true, projectName: 'pipeline_unit_tests_unix', selector: downstream(upstreamBuildNumber: '${BUILD_NUMBER}', upstreamProjectName: 'test_deploy'), target: '${WORKSPACE}'
    //
    //                     sh 'ls ${WORKSPACE}'*/
    //                 }
    //             }
    //
    //         }
    //     }
    // }
    // post {
    //     success {
    //         setGitHubPullRequestStatus context: 'Jenkins CI Pipeline', message: 'The Jenkins CI passed', state: 'SUCCESS'
    //     }
    //     failure {
    //         setGitHubPullRequestStatus context: 'Jenkins CI Pipeline', message: 'The Jenkins CI build failed', state: 'ERROR'
    //     }
    //     unstable {
    //         setGitHubPullRequestStatus context: 'Jenkins CI Pipeline', message: 'The Jenkins CI tests failed', state: 'FAILURE'
    //     }
    }
}
