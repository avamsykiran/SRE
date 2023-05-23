SRE - Principals , Pillars of SRE, DevOPS   1 Session
Versioning control - GitHub and Bitbucket   2 Sessions
CI/CD pipeline tools                        3 Sessions
Docker/Containerization                     2 Sessions
Amazon - EKS                                2 Sessions

Versioning Control
----------------------------------------------------------------------------------------

    allows us to have a central code repository, and here the team will be able
    to share, accept or reject the changes made to the code base and accordingly
    the code base is versioned.

    Repository
        |- Local Repository         is a code base monitored by a SVM tool.
        |- Remote Repository        the code base can move to a central location or back from it, and
                                    that called the remote repository.

                a. create a remote repository
                b. this remote repository will have a basic project strucutre.
                c. this basic project structure or the initial state of the repo is called 'main' branch.
                d. we can weave any number of branches where initially each branch is a copy of its parent.
                e. a branch can have child branches and they can have chuld branches each and so on.

                    main
                        |- b1   (sales modeule)
                            |-b11   (Vamsy)
                            |-b12   (Sagar)
                        |- b2   (delivery module)
                            |-b21   (Dheraj)
                            |-b22   (Prathap)
                        |- b3   (accoutn module)
                            |-b31   (Jyothi)
                            |-b32   (Calvin)
                                .....etc.,

                each branch may represent each module of the app, or each team member of the app and so..on.
            
                f. Chck-out means clsoing a branch after pushing the modified code into it.
                g. the checking process has 
                        1. Add all thsoe files to be sent into a staging area. (STAGING)
                        2. Add or remove the fiels from the staging area.
                        3. Commit the stage. (seal the stage after which no more adding or removing the files from staging)
                        4. We can commit any number of time but before each commit a staging happens.
                        5. Push means sending all the changes made from our local repo to the main repo.
                h. Cloning is where a remote repo is downlaoded into a our local disk and we call it the Local Repo.
                i. Pulling is where the changes made to the remote repo are downloaded into the local repo.
                j. Mergin is to accept the changes that already made to the remote repo and then accomidate the changes 
                    made on the local repo.
                
        Private vs Public Repositories

            Public Repositories allow anyone having the repo url to clone and pull.
            But only invited collaborators are allowed to push the content.

            Private Repo only invited collaborators are allowed to access of any kind.

        git-cli has to be installed.

DevOps

    Development and Operations

    is a large set of tools and servers that automate the Developemnt to operations to testing to deployment activities.

    the entier mail triggering and manual intervention of the developer and operations team automated .. HOW?

    CI / CD     ---------------> Continous Integration and Continous Development.

    CI / CD is performed by a CI/CD server, like Jenkins/ BlueCloud / BlueOcean .....etc.,


Jenkins Job Configuaration Stages
---------------------------------------------------
    1. Job Description
    2. Source Code Management
    3. Pre-Build Stage
    4. Build Stage
    5. Post-Build Stage

Jenkins Pipeline Job
---------------------------------------------------
    Pipeline
                is a sequence of stages (generally used in the production assembly line)
                where the outcome of one stage is input to the next stage,
                and is natured such that these stages are isolated and can be added or removed 
                from the pipeline.

                A new made empty bottle------------------------------>  |
                                                                        | - filling -> caping -> lableing -> packing -Dispatch
                A collected sued bootle--> Cleans and Sanitigation--->  |


    1. Jenkisn pipeline job can be created using a pipeline script file.
    2. Groovy language is used to script the pipeline script file.
    3. Jenkinsfile is expected at the root of a project code base containing the pipeline script.

        Pipeline
            a collection of seqeunced stages
                and each stage is a collection of sequenced steps.

        pipeline {
            agent any //execute this job on any avaible jenkins executor....

            tools {
                //JDK or maven or ant or gradle or sonarscanner ...etc tools can be mentioend here...!
                // Install the Maven version configured as "M3" and add it to the path.
                maven "M3"
            }

            stages {
                stage('CQA'){
                    steps{
                        //execute soanrscanner
                    }
                }
                stage('Build') {
                
                    steps {
                        // Get some code from a GitHub repository
                        git 'git repo url'

                        // Run Maven on a Unix agent.
                        sh "mvn -Dmaven.test.failure.ignore=true clean package"

                        // To run Maven on a Windows agent, use
                        // bat "mvn -Dmaven.test.failure.ignore=true clean package"
                    }

                    post {
                        // If Maven was able to run the tests, even if some of the test
                        // failed, record the test results and archive the jar file.
                        success {
                            junit '**/target/surefire-reports/TEST-*.xml'
                            archiveArtifacts 'target/*.jar'
                        }
                    }
                }
            }
        }

    Pipeline blocks:
    -------------------------------------------------------------------------

        agent/node

            on which executor should a job execute.

            agent any
            node any
                any avaialble node/agent executor can be assigned with our job.
            
            if we have multiple agents with each agent having a 'label' to identify then,

            agent{
                label 'window machine 1'
            }

        paramters {}        block is used to crate parameter which can be supplied a value by the job-user.

            parameters{
                string(name='buildFileName',desc='give a name ot the build file')
                text(name="description',desc='give a description')
                booleanParam(name="isProductionMode")
            }

        environment {
           JAVA_HOME 'c:/program files/java/jdk1.12'
        }

        tools {
            jdk 'default'
            mavne 'M3'
        }
        
        stages{
            stage('check tools'){
                bat 'echo ${JAVA_HOME}'
                bat 'java --version'
                bat 'mvn --version'
            }
            stage('build'){
                bat 'mvn clean package'
                bat 'cd ./target'
                bat 'ren emp-api-**.jar ${params.buildFileName}'
                bat 'cd..'
            }
        }

    https://www.lambdatest.com/blog/jenkins-declarative-pipeline-examples/
    
Docker
---------------------------------------------------------------------------------------------------------------

    is a software containerization platform. engages in build, deploy and run strategy.

    Virtualization

        1. The hardware owner ship cost is very high and support different OS on different machines is a fianancial 
            chanllenge and an operational challenge.
        
        2. Vitualization offers a solution by allowing to run multiple operting system on the same machine.

        3. Three Advantages
            a) multiple OS on the smae machine
            b) operetionl convinience like maintanence and recovery.
            c) owner cost is reduced.

        4. Drawbacks
            a) Each OS will have to occupy its share of the hardware
            b) This leaves a very little room for the actual app to execute
            c) that is hampaering the app performence.

    Containerization

        is virtualization with hardware and kernal abstraction.

        a cotnaienr will carry the binaries and libraries and the app.

        the calls made by the app to its designed OS are ansered by the contianer engien and
        are executed by the Host OS.

        1. very light weught
        2. fast and manageble.

    Container = app + libraries + binaries 

    ** binaries contiane the OS abstraction layer as well.

    Features Of Docker

        1. docker offer small OS footprints to cater the OS abstraction layer.

        2. docker does not demand the usage of the OS abstraction offerd by docker alone, it supports
                third party designed OS abstractions as well.

        3. docker offers its deployment on a varity of instrucments like eith on a physical machine,
            or on a virual machien or on a cloud ..et.,

        4. docker contianers are very light weight and are scalble.
