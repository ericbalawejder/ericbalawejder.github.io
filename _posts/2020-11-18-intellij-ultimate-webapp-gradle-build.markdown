---
layout: page
title:  "IntelliJ Java Webapp Gradle Build Failure"
date:   2020-11-18 12:00:00 -0000
categories: jekyll update
---

I'm using the IntelliJ Ultimate IDE. I won a one year license at a [PhillyJUG](https://www.meetup.com/PhillyJUG/) meetup raffle. The Ultimate edition provides server integration, a significant amount of framework support as well as a team of support people to resolve your issues that the community edition does not have. You can view a [comparison](https://www.jetbrains.com/idea/features/editions_comparison_matrix.html). The server support is a mere convenience and isn’t required. It does create a more pleasant experience over the verbose Tomcat command line server arguments or the gradle configuration.

I keep getting burned every time I create a new Java webapp project in IntelliJ. When deploying the war file to the server, IntelliJ can not find it. I forget this issue exists because I don't create Java webapps that often. I'm using Spring Boot or Dropwizard mostly with a built in server. I end up wasting an hour or so trying to understand the problem until I remember the issue. It is a documented issue with the default gradle build process. The issue can be found [here](https://youtrack.jetbrains.com/issue/IDEA-176700), as well as a [work around](https://youtrack.jetbrains.com/issue/IDEA-178450#focus=streamItem-27-4068591.0-0), but it hasn’t been integrated into updated releases for some reason. The comments on the issue page echo this. The first time this happened, I dug for hours before I ended up opening a support ticket. The team resolved my issue within a few hours. Below is an illustration of the steps.

<br>
Initial setup structure.
![project structure](/assets/intellij/project-structure.png)


Adding Tomcat configuration with the exploded war artifact.
![server setup](/assets/intellij/server-setup.png)


The war file can not be found with the default gradle build setting.
```
[2020-11-18 08:13:42,572] Artifact Gradle : org.example : spring-rest-demo-1.0-SNAPSHOT.war (exploded): com.intellij.javaee.oss.admin.jmx.JmxAdminException: com.intellij.execution.ExecutionException: /Users/ericbalawejder/Development/spring-rest-demo/build/libs/exploded/spring-rest-demo-1.0-SNAPSHOT.war not found for the web module.
```
Select the following menu settings:
<br>
`IntelliJ IDEA -> Preferences -> Build, Execution, Deployment -> Build Tools -> Gradle`
![gradle default](/assets/intellij/gradle-default.png)


Change to build and run with IntelliJ.
![build with intellij](/assets/intellij/build-with-intellij.png)

```
Connected to server
[2020-11-18 08:20:25,110] Artifact Gradle : org.example : spring-rest-demo-1.0-SNAPSHOT.war (exploded): Artifact is being deployed, please wait...
[2020-11-18 08:20:25,564] Artifact Gradle : org.example : spring-rest-demo-1.0-SNAPSHOT.war (exploded): Artifact is deployed successfully
[2020-11-18 08:20:25,564] Artifact Gradle : org.example : spring-rest-demo-1.0-SNAPSHOT.war (exploded): Deploy took 454 milliseconds
```