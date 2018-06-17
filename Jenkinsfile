import hudson.triggers.TimerTrigger;
import jenkins.branch.BranchEventCause;
import jenkins.branch.BranchIndexingCause;
import org.jenkinsci.plugins.workflow.cps.replay.ReplayCause;

def determineCause()
{
    for (cause in currentBuild.rawBuild.getCauses())
    {
        if (cause instanceof BranchEventCause)
        {
            echo "Triggered by BranchEventCause!"
            return "branch"
        }
        else if (cause instanceof BranchIndexingCause)
        {
            echo "Triggered by BranchIndexingCause!"
            return "pr"
        }
        else if (cause instanceof TimerTrigger.TimerTriggerCause)
        {
            echo "Triggered by cron!"
            return "cron"
        }
        else
        {
            echo "Triggered by ReplayCause!"
            return "other"
        }
    }
}

pipeline {
    agent any
    triggers {  // Additional trigger to build nightly on master.
        cron('H/15 1 * * *')
    }
    environment {  // Prepare environment for build.
        PR_NUMBER = "${CHANGE_ID}"
        BUILD_TRIGGER = "${determineCause()}"
    }
    stages {
        stage ('Configure') {  // Do we need this?
            steps {
                script {
                    echo "My PR ${env.PR_NUMBER}"
                }
            }
        }
        stage ('Test') {
            parallel {
                stage ('unix unit') {
                    steps {
                        echo "Perform unit tests."
                    }
                }
                stage ('doc') {
                    steps {
                        echo "Perform doc tests."
                    }
                }
                stage ('memory') {
                    steps {
                        echo "Perform memory tests."
                    }
                }
                stage ('coverage') {
                    steps {
                        echo "Perform coverage tests."
                    }
                }
                stage ('performance') {
                    steps {
                        echo "Perform performance tests."
                    }
                }
            }
        }
        stage ('Deploy') {
            agent none
            when {
                beforeAgent true
                branch 'PR-5'
                anyOf {
                    environment name: 'BUILD_TRIGGER', value: 'cron'
                    tag "seqan-*"
                }
            }
            stages {
                stage ('package') {
                    steps {
                        script {
                            if (env.BUILD_TRIGGER.equalsIgnoreCase("cron"))
                            {
                                echo "Deploy nightly stable."
                            }
                            else
                            {
                                echo "Deploy release."
                            }
                        }
                    }
                }
                stage ('acceptance test') {
                    steps {
                        echo "perform acceptance test"
                    }
                }
            }
        }
    }
}
