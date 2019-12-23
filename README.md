# ssl-client-certificate-validator
Custom policy for mule 4 to validate SSL client certificate parameters in HTTP headers (APIGW 2.x)



Custom Policies are policies that anyone can develop and apply to their APIs, with the intention of extending existing functionality or defining new ones.


The current workflow to get a working policy for Mule 4 that can be applied in Anypoint Platform consists of:

1.Develop the policy.

2.Package the policy.

3.Upload the resulting policy assets to Exchange.

4.Apply the policy to any API through API Manager.



You can clone this respository to implement ssl-client-certificate-validation policy in mule 4.


Replace:

◦xxxx with the Anypoint Platform Organization Id where the policy will be uploaded.

◦Get your organization ID from Access Management > Organization:

◦Click the name of your organization.

◦Copy the UUID from the browser address. For example, copy 2a4b93c3-7899-4ea7-9374-f787744d8784 from the URL.




This repository contains the basic four files needed to have a working ssl-client-certificate-validation policy in mule 4.


1) pom.xml


◦groupId is defined as the organization ID used with the archetype. This value must remain as it is.


◦mule-policy packaging, so packager plugin can successfully build the JAR.


◦distributionManagement section is defined pointing to user’s Exchange.


◦com.google.code.maven-replacer-plugin:replacer used to replace values in mule-artifact.json that should always match values defined in this pom file.


◦mule-maven-plugin responsible for packaging the policy into a deployable jar


◦maven-deploy-plugin configured to deploy both the resulting jar and the YAML when uploading the policy to Exchange



2) mule-artifact.json exists for the mule-maven-plugin. This is the same file you need for Mule applications.


3) ssl-client-certificate-validator.yaml renders the policy configuration UI. If this file is not provided, the policy won’t be able to be applied through API Platform’s UI.

See (UI-API-Platform file) for "API Platform’s UI"



4) template.xml where the actual logic of the policy and Mule configuration that defines the policy behavior.






Packaging a Custom Policy






You can use the Mule Maven Plugin to package your custom policy into a deployable JAR file that you can later apply to an API.


1.Make sure you added the Mule Maven Plugin to your pom.xml file. 


    <plugin>
    <groupId>org.mule.tools.maven</groupId>
    <artifactId>mule-maven-plugin</artifactId>
    <version>${mule.maven.plugin.version}</version>
    <extensions>true</extensions>
    </plugin>


2.From the command line in your project’s folder, run the install phase:


    > mvn clean install




The packager then packages your application and creates the deployable JAR file into the target directory within your project’s folder.


It will also verify that all the necessary files were provided for the packaging, and that the information provided in the mule-artifact and in the policy yaml are valid.



Uploading the Policy to Exchange :


You must configure your Exchange credentials:


1. Update the settings.xml with your Exchange credentials.


    <?xml version="1.0" encoding="UTF-8"?>
        <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
        <servers>
            <server>
            <id>exchange-server</id>
            <username>myusername</username>
            <password>mypassword</password>
            </server>
        </servers>
        </settings>


2. Run  mvn deploy   to publish the policy to Exchange.



The custom policy is now available for you to apply to APIs that belong to the specified organization.



