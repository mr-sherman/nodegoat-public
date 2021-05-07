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
