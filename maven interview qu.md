1. what is the call back plugins are used in ansible?
A. In Ansible, callback plugins are used to customize and enhance the output and logging of playbook execution.
PluginName	  n  Description
default	        Standard human-readable output
yaml	        YAML-formatted output
json	        JSON-formatted output
minimal	        Less verbose output
debug	        Debugging-focused output
log_plays	     Logs playbook output to a file
profile_tasks	 Measures execution time of tasks
slack	         Sends notifications to Slack
syslog	         Sends logs to syslog

a. Enable a plugin globally by modifying ansible.cfg:
[defaults]
callback_whitelist = profile_tasks, yaml

b.Enable a plugin for a single execution:
ANSIBLE_STDOUT_CALLBACK=json ansible-playbook playbook.yml


2. maven architecture?
Maven follows a three-tier architecture consisting of the following key components:
a.Build Lifecycle & POM (Project Object Model):
Maven defines a standard build lifecycle that consists of multiple phases (such as validate, compile, test, package, etc.).
The POM.xml (Project Object Model) is the core configuration file where dependencies, plugins, and build configurations are defined.
b. Repositories (Local, Central, and Remote)
Maven retrieves dependencies and plugins from repositories:

Local Repository (~/.m2/repository/) → Cached dependencies on your system.
Central Repository (Maven Central - https://repo.maven.apache.org/maven2/) → Default public repository.
Remote Repository (Custom Repos like Nexus, Artifactory) → Private or third-party repositories.

c. Plugin & Execution Engine
Maven uses plugins to perform tasks (e.g., maven-compiler-plugin for compiling Java code).
The Maven Execution Engine executes lifecycle phases using these plugins.

Maven Workflow (How it Works):
User runs a command, e.g., mvn clean install.
Maven reads the pom.xml to understand dependencies, plugins, and configurations.
Maven resolves dependencies from the local repository.
If dependencies are missing, it downloads them from the Central/Remote repository.
Maven executes the build lifecycle in the defined sequence.
Artifacts are generated (JAR, WAR, etc.) and stored in the local repository.

3. how to u check maven version and maven artifact?

maven --version

maven artifact: Check Maven Artifact in Local Repository:

Search for an Artifact in the Local Repository
To check if a specific Maven artifact exists in your local repository (~/.m2/repository/), use:

**mvn dependency:get -Dartifact=groupId:artifactId:version

Check Maven Artifact in Remote Repository:

mvn dependency:resolve -Dartifact=groupId:artifactId:version

4. why are maven plugins are used?

Maven plugins are used to execute specific tasks during the build lifecycle of a project. They extend Maven’s capabilities, enabling compilation, testing, packaging, deployment, reporting, and more.

Automate Build Tasks,manage dependecies efficiently, standardize the build lifecycle, Customizable & Extensible,Integrate with CI/CD Pipelines

Maven plugins are categorized into two types:
1. Build Plugins (Execution During Build Lifecycle)
maven-compiler-plugin → Compiles Java source code.
maven-surefire-plugin → Runs unit tests.
maven-jar-plugin → Packages the project as a JAR file.
maven-war-plugin → Packages the project as a WAR file.

2. Reporting Plugins (Generate Reports & Documentation)
maven-site-plugin → Generates a project documentation site.
maven-checkstyle-plugin → Checks code quality.
maven-javadoc-plugin → Generates Javadoc documentation.

How to Run a Maven Plugin: maven clean compile.


5. what is the maven order of inheritence?

In Maven, inheritance follows a structured order that determines how configurations, dependencies, and plugins are inherited across different POM files

1.The Super POM is the default parent POM that all Maven projects inherit from.
It provides default configurations such as:
Default repositories (Maven Central)
Default plugins
Default lifecycle phases

    Located at:
    $MAVEN_HOME/lib/maven-model-builder-*.jar

        2. Parent POM (Custom Parent):

        A project can define a custom parent POM, which allows multiple child projects to inherit common settings, dependencies, and plugin configurations.

        3. Child POM (Project-Specific Configuration)
        Child projects inherit settings from the Parent POM but can override specific configurations.
        If a value is not overridden, it is inherited from the parent.

        <parent>
            <groupId>com.example</groupId>
            <artifactId>my-parent-project</artifactId>
            <version>1.0.0</version>
        </parent>

        <artifactId>my-child-project</artifactId>

        or 
    <project xmlns="http://maven.apache.org/POM/4.0.0" 
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
            http://maven.apache.org/xsd/maven-4.0.0.xsd">
        
        <modelVersion>4.0.0</modelVersion>

        <groupId>com.example.parent</groupId>
        <artifactId>parent-project</artifactId>
        <version>1.0-SNAPSHOT</version>
        <packaging>pom</packaging>

        <modules>
            <module>child-project1</module>
            <module>child-project2</module>
        </modules>

    </project>



        4. Dependency Management (Overriding Dependencies)
        The parent POM can define dependency versions using the <dependencyManagement> section.
        The child POM can use these dependencies without specifying versions.



6. What Are Dependencies in Maven? and repository in maven?
In Maven, dependencies are external libraries (JAR files) required by a project to compile and run properly. Instead of manually downloading JAR files, Maven automatically fetches them from repositories.

Types of Dependencies
Compile Dependency (Default) → Required at compile time and runtime.
Provided Dependency → Required at compile time but provided by the runtime environment (e.g., Servlet API).
Runtime Dependency → Needed only at runtime, not for compilation.
Test Dependency → Required only for running tests (e.g., JUnit).
Optional Dependency → Not included unless explicitly needed.


A repository is a storage location where Maven downloads and stores dependencies (JAR files). There are three types:

A. Local Repository (~/.m2/repository/)
B. Central Repository (Maven Central)
Default online repository managed by Maven.
c. Remote Repository (Private or Custom)
Used by organizations to host private dependencies.
Examples: Nexus Repository, Artifactory.


7.what happend if the dependices are not accepting in local repository  and what maven will do
Maven Central Repository (default) → https://repo.maven.apache.org/maven2/
Any custom remote repositories defined in pom.xml or settings.xml.

mvn clean install -U
🔹 -U forces Maven to update dependencies from remote repositories.

what is the snapshot in maven?


🔹 1.0.0-SNAPSHOT means this is a work-in-progress version that may change frequently.

Feature	           SNAPSHOT Version (1.0.0-SNAPSHOT)	           Release Version (1.0.0)
Purpose	            Used during development	                       Final, stable version
Updates	             Changes frequently	                           Fixed and doesn’t change
Repository	         Stored in SNAPSHOT repository	               Stored in Release repository
Deployment	         Overwrites previous versions	               New unique version is required



Where Are SNAPSHOTs Stored?
Maven stores snapshots in special repositories separate from release repositories.



What Is a Build Profile in Maven?
A Maven Build Profile is a way to customize the build process based on different environments (e.g., development, testing, production). Profiles allow you to override settings, such as dependencies, plugins, and configurations, based on conditions.

 Use Build Profiles:
Profiles help in situations like: ✅ Running different configurations for development, testing, and production.
✅ Using different databases (e.g., H2 for dev, MySQL for production).
✅ Deploying the application to different servers.
✅ Managing optional dependencies based on the environment.


<profiles>
    <!-- Development Profile -->
    <profile>
        <id>dev</id>   (define profile name )
        <properties>
            <env>development</env> (define your env specific varaibles)
        </properties>
    </profile>

    <!-- Production Profile -->
    <profile>
        <id>prod</id>
        <properties>
            <env>production</env>
        </properties>
    </profile>
</profiles>

 Activating a Profile: mvn clean install -Pdev
                       mvn clean install -Pprod

14. what is pom.xml file?
Section	Purpose
<groupId>	Unique ID of the project’s group (usually your company or organization)
<artifactId>	The name of the project
<version>	Project version
<dependencies>	External libraries your project needs
<build>	How to compile/package the project, including plugins
<properties>	Key-value pairs for settings (like Java version)
<repositories>	Optional. Add custom repo URLs if dependencies aren't on Maven Central





 1. Declarative Pipeline
Declarative pipelines are the preferred and more structured way to define Jenkins pipelines. They use a specific syntax with predefined blocks and are easier to read and maintain.

✅ Key Features:
Easier to write and understand

Has predefined sections like stages, steps, environment, etc.

Supports post-build actions (post block)

Validation and syntax checking


. Scripted Pipeline (Descriptive)
Also known as descriptive or imperative pipeline. It's more flexible but requires full Groovy scripting knowledge. Best used for complex workflows.

🛠️ Key Features:
Full control with Groovy scripting

More flexible, less structured

Better for complex use cases (like loops, conditionals)



*** Parallel execution in Jenkins pipelines lets you run multiple stages or steps simultaneously—great for speeding up builds and tests. 💨
All stages inside parallel block will run at the same time.

**If one fails, the whole stage is marked as failed (unless you handle it).

**use try-catch block inside that parllel block of stages it will handle.

**Use catchError or unstable if you want to continue but mark build unstable.

**Use failFast: true in declarative pipeline to fail early when one parallel branch fails:

