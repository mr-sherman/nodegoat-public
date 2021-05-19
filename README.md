# GitHub Advanced Security Featuring NodeGoat

## Please Read:

This repository exists as a way of demonstrating how GitHub Advanced Security fits within a development process in GitHub.  It is not meant to be built, deployed, hacked, or used in any kind of CTF.  It is a fork of the original NodeGoat.  If you want to try to learn how to hack NodeGoat, go here:
[ https://github.com/OWASP/NodeGoat ]

### Learn how GHAS integrates into a typical developer workflow.  To start, fork this repository.

### First, let's fix the code injection vulnerabilities.
  - In the "security" tab, click "Code scanning alerts", and find the Code Injection vulnerabilties.  There are three of them, but they all originate in the same file:  app/routes/contributions.js 
  - Select one, walk through the path by clicking "Show Path", and click on the file name to bring you to the line of code that is the source of the vulnerability.  
  - Change the branch to "Master" from the branch pull down list, and click the pencil icon to edit the file.  
  - Comment out lines 32, 33, and 34.  
  - Uncomment out lines 38, 39, and 40.
  - Create a branch and pull request, and click "Propose Changes".

This process will start the Code Scanning process.  All checks should pass, even though security vulnerabilities still exist.  This because this branch does not introduce any new vulnerabilities. Once all the checks pass, merge the changes.

This will start another code scanning process on the newly merged master branch.  Once the code scanning process is complete, then the three code injection vulnerabilities will close.

You can verify this by going into the code scanning alerts screen from the Security tab, and filtering for closed vulnerabilities.  

### Next, let's introduce a vulnerability.
  - Go to the file app/routes/profile.js
  - Comment out line 59
  - Uncomment out line 58
  - Create a new branch and pull request and click "Propose Changes".
  - This will start another code scanning process.  The check will fail because this branch introduces new security vulnerabilities, in this instance an "Inefficient Regular Expression".


### Next, let's fix a vulnerable open source dependency. 
   - Go the Security tab and select "Dependabot Alerts"
   - Find the vulnerable package lodash with a high severity vulnerability, and select that.
   - Click "Create Dependabot security update"
   - A pull request will get created - this process may take some time.
   - Merge the pull request to close the dependabot alert.
   
## Advanced GHAS Concepts
The following steps are for security champions, DevSecops personnel or people interested in learning more about CodeQL as a language and tool for security research.
Before continuing, go to the  [VSCode CodeQL Starter Repo](https://github.com/github/vscode-codeql-starter).  
 - Follow the instructions in the README.md  there to install the Visual Studio Code extension and set up a starter project for developing your own CodeQL queries.
 - Download the [CodeQL CLI](https://codeql.github.com/docs/codeql-cli/getting-started-with-the-codeql-cli/) so you will be able to create a CodeQL database of NodeGoat
 - Create a CodeQL Database for your version of the NodeGoat source code.
  - From the root of your Nodegoat source code directory Run the command:   codeql database create nodegoat-codeql-db --language=javascript
  - This will create a folder in your NodeGoat root source directory called "nodegoat-codeql-db"
 - In the Visual Studio CodeQL Starter Workspace you created in step one, enter the command pallette and type "CodeQL:"  Select "CodeQL:  Choose Database From Folder". 
 - Navigate to the database folder that was created, "nodegoat-codeql-db".
 - In Visual Studio Code, go to the CodeQL view.  In the "Databases" section, right click on the nodegoat database and select "Set Current Database"
 - Create a new file in the starter workspace called "sensitive-info.ql"
 - At the top of the file type "import java".  You're ready to begin.


### Let's hunt for more vulnerabilities:
  - Your organization wants to meet an industry regulation that states all sensitive user information must be encrypted.  First, we have to find instances where sensitive info exists in the code.
  - Enter the following query into your sensitive-info.ql file: ```import javascript
import DataFlow::PathGraph

from  PropAccess pa where pa.getPropertyName().toLowerCase().regexpMatch(".*ssn")
or pa.getPropertyName().toLowerCase().regexpMatch(".*dob")
select pa```
  - Right click on anywhere in the file and select "CodeQL:  Run Query."  This will find all uses of sensitive information like social security number (SSN) and date of birth (DOB)
 
