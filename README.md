# SAP S/4HANA Cloud SDK Pipeline

![Visualisation of SAP S/4HANA Cloud SDK Pipeline](images/s4sdk-pipeline.gif)

## Description

The [SAP S/4HANA Cloud SDK](https://sap.com/s4sdk) helps to develop S/4HANA extension application on the SAP Cloud Platform. 
Continuous integration and delivery (CI/CD) is an important aspect of cloud application development.
This repository contains a Jenkins pipeline as code designed for the requirements and structure of application written with this SDK. 
It contains the steps for building, testing and deploying applications to the SAP Cloud Platform.

## Requirements

### Hardware requirements

At least four gigabyte memory (available to Docker) and at least four gigabyte of disk space on the Jenkins master for downloading Docker images and the persistent storage of Jenkins.

The pipeline will refuse to run with less than one gigabyte available disk space to prevent a situation where the disk is running full.

### Software requirements

To use the pipeline you need a Jenkins server which has the [pipeline library](https://github.com/SAP/cloud-s4-sdk-pipeline-lib) as shared library configured.
The best way to achieve this is to use the SAP S/4HANA Cloud SDK Cx Server.

For instantiating the SAP S/4HANA Cloud SDK Cx Server, you need to provide a suitable host with a Linux operating system and Docker installed.
Please also ensure that the user with whom you start the Cx Server belongs to the `docker` group.

Your project source files need to be available on a git or GitHub server, which is accessible from the Cx Server host.

The lifecycle of the Cx Server is maintained by a script called `cx-server`.
It can be found in the same named folder on the root of each SAP S/4HANA Cloud SDK project archetype. Together with the `server.cfg` file, this is all you need for starting your instance of the SAP S/4HANA Cloud SDK Cx Server.

You might chose between the available archetypes, depending on the technology you prefer:

- `scp-cf-spring`
- `scp-cf-tomcat`
- `scp-cf-tomee`
- `scp-neo-javaee7`

To create a new project using the SDK execute the following command:

```shell
mvn archetype:generate -DarchetypeGroupId=com.sap.cloud.s4hana.archetypes -DarchetypeArtifactId=scp-cf-tomee -DarchetypeVersion=RELEASE
```

To use one of the other archetypes, replace the value of `DarchetypeArtifactId` accordingly.

In the new project, there is a folder called `cx-server`.
This folder needs to be copied to the future host on which the Cx Server is intended to run.

On the host machine, navigate to the `cx-server` directory and ensure that the `cx-server` file is executable.
After that you can start the Jenkins server with following command:

```shell
./cx-server start
```

In Jenkins, click on "New Item" and create a new "Multi-branch Pipeline" for your repository.  

## Download and Installation

In order to use the pipeline just load the pipeline within your `Jenkinsfile` that is placed in the root of your project repository. 
Create a file called `Jenkinsfile` and add the following example code:

```groovy
#!/usr/bin/env groovy 

node {
    deleteDir()
    sh "git clone --depth 1 https://github.com/SAP/cloud-s4-sdk-pipeline.git pipelines"
    load './pipelines/s4sdk-pipeline.groovy'
}
```

After you commit your changes and the Jenkins server starts to build the project it will automatically use the pipeline. 

## Blog Posts
In order to learn more about the SAP S/4HANA Cloud SDK Continuous Delivery pipeline, feel free to read our [blog post](https://blogs.sap.com/2017/09/20/continuous-integration-and-delivery).

## Analytics
To improve SAP S/4HANA Cloud SDK Pipeline, we do collect non-personal telemetry data.

For details about which data is collected, and for information on how to opt-out, consult the [analytics documentation](doc/operations/analytics.md).


## Known Issues
Known issues are collected in the [GitHub issues](https://github.com/sap/cloud-s4-sdk-pipeline/issues).
If you encounter an issue, please report it there.
Be sure to remove any confidential information before.

## How to obtain support
If you need any support, have any question or have found a bug, please report it as an issue in the repository.

## License
Copyright (c) 2017-2019 SAP SE or an SAP affiliate company. All rights reserved.
This file is licensed under the Apache Software License, v. 2 except as noted otherwise in the [LICENSE file](LICENSE).
