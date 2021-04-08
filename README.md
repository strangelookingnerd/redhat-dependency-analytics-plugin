# crda-dependency-analysis
CRDA dependency analysis is a jenkins plugin developed by **Red Hat Inc**. CRDA is a platform which acts as a one stop solution to get the details of your dependency stack. It gives you details on Vulnerability (**Powered by Snyk**), License Compatibility, Popularity, Maintainability and much more.

The plugin can be used in jenkins as a pipeline task or as a build step.

## How to use crda dependency analysis plugin
### Admin Steps
### 1.  Generate CRDA Key
- Download the CRDA CLI tool on your system. Click [here](https://github.com/fabric8-analytics/cli-tools/releases "here") to download.
- Follow the instructions for the installation and run `crda auth` command to generate the crda key. Copy this key.
- Compatible CLI versions >= v0.1.0

### 2. Install the crda-dependency-analysis jenkins plugin
- Goto the jenkins dashboard -> Manage Jenkins -> Manage Plugins.
- Seach for `crda-dependency-analysis` and install.

### 3. CRDA credentials
- Goto jenkins dashboard -> Manage Jenkins -> Manage Credentials. Select the global domain -> Add Credentials.
- Select the option `CRDA Key` in the Kind option.
- Let the scope be global.
- Enter a valid CRDA Key which was generated via the crda auth command using the CRDA CLI.
- Let the ID field blank. Jenkins will generate the ID.
- Give some description for the identification of the credentials.
![Screenshot from 2021-04-08 13-37-05](https://user-images.githubusercontent.com/37098367/114042617-361ea000-98a3-11eb-9740-6849b9d7593b.png)

### 4- Configuration
#### Option I- As a build step
- Click on Configure -> Build Trigger -> Add Build Step. Select `Invoke CRDA Analysis`.
- Filepath (Mandatory): Provide the filepath for the manifest file. We currently support the following
	- Maven: pom.xml
	- Python: requirements.txt
	- Npm: package.json
	- Golang: go.mod
- CRDA Key Id (Mandatory): The Id generated by jenkins from the step 3.
- CRDA CLI Version (Optional): The crda cli version that you need to run underneath. (Accepted format example v0.1.0). It will pick the default value [v0.1.0] if no value is provided.
![Screenshot from 2021-04-08 16-54-29](https://user-images.githubusercontent.com/37098367/114042741-52224180-98a3-11eb-8844-ea6921f4ef23.png)

#### Option II- As a pipeline task
- Its just a single line that you need to add in your pipeline script.
`crdaAnalysis file:'manifest file path', crdaKeyId:'crda key id', cliVersion:'version'`
The value description remains the same as provided in the Option I.
- It returns 3 different exit status code
	- 0: Analysis is successful and there were no vulnerabilities found in the dependency stack.
	- 1: Analysis encountered an error.
	- 2: Analysis is successful and it found 1 or more vulnerabilities in the dependency stack.

## Results
There are a total 3 ways to view the results of the analysis.
### 1. Console Output
This provides the count and types of vulnerabilities found in the dependency stack. This data is generated for every build and can be viewed in the corresponding console log. It also provides a link to the detailed report.
![Screenshot from 2021-04-08 18-14-36](https://user-images.githubusercontent.com/37098367/114042855-6bc38900-98a3-11eb-9ae9-e2e956553a6e.png)

### 2. CRDA Stack Report
After every successful analysis, you can find a new icon added in the left panel named
`CRDA Stack Report` . Click on this icon to view the report in graphical form. Here too, we provide a button to redirect to the detailed stack report UI.
![Screenshot from 2021-04-08 18-18-23](https://user-images.githubusercontent.com/37098367/114042990-87c72a80-98a3-11eb-8fe5-d577a119cbcb.png)

### 3. Detailed CRDA Stack Report
The stack report can be accessed via 2 ways, as mentioned in point number 1 (via url) and 2 (via button click). The report provides comprehensive details about each vulnerability, each dependency in the stack along with the license analysis and the recommended companions.
![reg-stack-analysis](https://user-images.githubusercontent.com/37098367/114043532-002deb80-98a4-11eb-9c1d-5576c10af79e.gif)

## Snyk Registration
There are 2 ways to register with Snyk
### 1. Via CLI
If you have a Snyk token, then the same can be used at the time of `crda auth` cli command execution by providing the snyk token.

### 2. Via CRDA Stack Report UI
Follow the 2 steps shown below to generate a snyk token and provide it in the appropriate place in the stack report.

**Step1**
![snyk-sign-up](https://user-images.githubusercontent.com/37098367/114044101-86e2c880-98a4-11eb-83db-5e1e07bbae15.gif)

**Step2**
![snyk-token](https://user-images.githubusercontent.com/37098367/114044134-8f3b0380-98a4-11eb-84c0-70809a3cccc3.gif)
