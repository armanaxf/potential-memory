---
title: Use the Set Solution version task Power Platform Build tools task in Azure Devops
description: Learn how to use the Set Solution version task in the Power Platform build Tools and leverage it across different environments
slug: Use-the-Set-Solution-version-task-Power-Platform-Build-tools-task-in-Azure Devops
date: 2023-03-10 00:00:00+0000
image: software.jpg
categories:
    - Azure DevOps
    - Power Platform
    - Solutions
tags:
    - Power Platform
    - DevOps
    - Solutions
    - Pipelines
    - YAML
    - Verisoning
---

Power Platform Build tools offer a great way to manage Continuous Integration and Continuous Deployment(CI/CD) within Azure DevOps. Storing Solutions as code and working collaboratively using these tools is just some of the many benefits offered by making use of these tools and forms a key part of the overall Application Lifecycle Management(ALM). See https://learn.microsoft.com/en-us/power-platform/alm/(https://learn.microsoft.com/en-us/power-platform/alm/ for a more in-depth overview of ALM for Power platform.

When exporting a solution from an Environment, it is good practice to set  solution version to enable solutions to be tracked more easily and track what functionality is included in a release.

Up until recently, I had run into a bit of a roadblock where I was unable to set a solution version when using a service connection which had a different default environment set compared to the environment I was wanting to target. After much head scratching, I actually found that although the build tools do not specify that you can set an Environment to target, you can in fact target an Environment if you specify the parameter and ignore the warning!

Here's an example of the task:

~~~Yaml
- task: PowerPlatformSetSolutionVersion@2
  inputs:
    authenticationType: 'PowerPlatformSPN'
    PowerPlatformSPN: 'My Service Connection (Default)'
    Environment: ${{ parameters.Environment }}
    SolutionName: ${{ variables.SolutionName}}
    SolutionVersionNumber: ${{ variables.VersionNumber}}
~~~

What we see above is the addition of the "Environment" Parameter which is not specified in documentation and when you add it to your task it will give the squiggly line of doom. To provide further context, I am specifying my Environment as a runtime parameter and my Solution Name as a variable. My version number is set by using a variable and specifying the Azure DevOps build number in the pipeline: $(Build.BuildNumber). My Environments have the Service connection (App registration) already added to them with relevant permissions, so that I can target them with no issues.

I've raised an issue on the Power Platform Build tools so hopefully this will be fixed soon to make it more obvious that you can in fact target different environments with this task. You can view the issue [here](https://github.com/microsoft/powerplatform-build-tools/issues/287)

You can read more about Parameters [here](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/runtime-parameters?view=azure-devops&tabs=script) and Variables [here](https://learn.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml)

To summarise, being able to set a the solution version of your exported solutions and targeting your environments is great as it's yet another aspect of CI/CD that you can configure.