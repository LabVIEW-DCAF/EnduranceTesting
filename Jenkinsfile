#!/usr/bin/env groovy

echo 'Starting pipeline'

node("proto"){
    echo 'Starting build...'
    stage ('Pre-Clean'){
        preClean()
    }
    stage ('Update Packages'){
        echo 'Updating all installed packages to latest version on internal repo'
        vipmUpdate("2014")
    }
	// If this change is a pull request and the DIFFING_PIC_REPO variable is set on the jenkins master, diff vis.
    if (env.CHANGE_ID && env.DIFFING_PIC_REPO) {
        stage ('Diff VIs'){
            lvDiff("2014")
        }
    }
    stage ('SCM_Checkout'){
        echo 'Attempting to get source from repo...'
        checkout scm
    }
    stage ('Temp Directories'){
        bat 'mkdir build_temp'
    }
    stage('cRIO EXE'){
        lvBuild("Automated Builds Project\\All Module Integration Test.lvproj", "cRIO9076", "", "2014")
    }
    // stage ('RT Tests'){
        // def config_file = "RT_Exec_Timing.pcfg"
        // def target_ip = "10.0.18.181"
        // def logfile = "logfile.tdms"
        // def logfile_path = "/home/lvuser/dcaf/${logfile}"
        // def user = "admin"
        // // Deploy config file
        // bat "echo y | pscp -pw  ${RIO_PASSWORD} ${WORKSPACE}\\RT-Test\\${config_file} ${user}@${target_ip}:/home/lvuser/${config_file}"
        // // delete log directory
        // bat "echo y | plink -pw ${RIO_PASSWORD} ${user}@${target_ip} rm -r -f /home/lvuser/dcaf"
        // // Run DCAF
        // bat "labview-cli --kill --lv-ver 2014 ${WORKSPACE}\\RT-Test\\Execute-RT-9068-Benchmarking.vi -- ${WORKSPACE} RT-Test\\DCAF-RT-Performance-Test.lvproj 9068-Main.vi cRIO home:\\lvuser\\${config_file} 60 ${target_ip}"
        // bat "echo y | pscp -pw ${RIO_PASSWORD} ${user}@${target_ip}:${logfile_path} ${WORKSPACE}\\${logfile}"
        // bat "labview-cli --kill --lv-ver 2014 \"${WORKSPACE}\\utilities\\Timing Report.vi\" -- \"${WORKSPACE}\" ${logfile} build_temp rt_exec_time.json ${BUILD_NUMBER} RT_Exec_Test"
    // }
    stage ('Post-Clean'){
        postClean()
    }
}
