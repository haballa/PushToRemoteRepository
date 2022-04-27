# PushToRemoteRepository

**Creation of back-up of a Power Automate Desktop flow (PAD) and pushing it to a remote repository (Git)**

-----------------------------------

## Pre-requisites
- a folder must be created named *exactly* like the PAD flow to be backed-up
- an environment variable with the name 'PADRobotsGeneralConfigPath' needs to be created; the value must be the folder where all the repositories of the PAD flows should stored
- the folder for each flow must a Git directory; this is not mandatory but the full functionality can only be leveraged this way
- since Git commands are used in the Powershell, the Powershell plugin posh-git must be installed --> https://www.pugetsystems.com/labs/hpc/Note-Setup-Git-for-PowerShell-on-Windows-10-1653/

-----------------------------------

## How to Use This Flow
**First run (initialization)**

If you start it for the very first time, a JSON file is copied to a folder named ".PowerAutomateDesktopRobotsConfig" in the folder specified in the above mentioned environment variable. After that, it is opened and you are prompted to change it to your needs. The flow will stop after the file is opened.

Change the entry "DefaultFolder" to the directory where all your flows are located. "WaitingTimeOpenEditor" only needs to be changed on very slow computers. The JSON must be saved manually. 

**Runs After Initialization Run**

If the JSON file already exists, the following steps are performed.

1. Select the flow to be backed-up. After that, no user intervention is needed until another form pops up.
2. If the PAD editor is not open, the robot opens the selected flow in the PAD console.
3. Once the flow is opened in the editor, the robot checks if there are any inputs and outputs defined in the flow. If so, those are written to the file "Inputs_Outputs.json". Depending on the number of inputs and outputs, this may take some time.
4. The robot copies the source code of each subflow and creates a text file for each subflow. These are stored in the subfolder "source_code". 
5. A form is opened providing some information. In addition, the change significance can be selected (see also https://en.wikipedia.org/wiki/Software_versioning#Semantic_versioning). The "description of changes" field determines the commit message and is also added to the version. In case, the local repository should be pushed to a remote repository (like Github or Gitlab), "Okay" must be pressed. "Cancel" ends the flow.
6. The "VersionHistory.txt" file is updated. 
7. The local repository is pushed to the remote repository. For major and minor changes (according to Semantic Versioning), a tag is created in Git.
8. A message box shows the Git message.
9. Flow is finished.

The time it takes from start to finish of the flow can vary from less than a minute (flow already opened in editor, no I/Os) to more than 3 minutes (flow not yet opened in editor, many I/Os).

-----------------------------------

## Description of Subflows
### Main
- Calling of "Z1_ConfigFile"
- Box for folder selection (= selection of flow)
- If necessary: editor for selected flow is opened
- Calling of "A0_InputsOutputs"
- Looping through all the subflows and copying of code to text files
- Calling of "B0_DeleteNonexistingTextfiles"
- Calculation of run duration so far
- Custom form: user input for version control and Git
- Calling of "C0_ReplaceCharacters"
- Calling of "D0_CreateNewVersionNumber"
- Git commands via Powershell
- Display of Git message

### A0_InputsOutputs
- Checking if there are any inputs/outputs
- If so, going through every I/O and copying data to JSON file

### B0_DeleteNonexistingTextfiles
- Deleting old source code text files

### C0_ReplaceCharacters
- Parsing the selected folder string and replacing characters that could cause trouble at Powershell (masking characters are added, if necessary)
- Parsing the commit message string and replacing characters that could cause trouble at Powershell (masking characters are added, if necessary)

### D0_CreateNewVersionNumber
- Creation of "VersionHistory.txt" file, if not existing
- Calculation of version increment based on the change significance

### Z1_ConfigFile
- Check if environment variable exists
- First run (initialization)
- Initialization of parameters

-----------------------------------

## Limitations
Due to way it works, there are some limitations to be considered. 
- Changes in the Power Automate Desktop version might affect the working of the flow. It interacts with the UI of PAD and changes in it could affect the flow negatively.
- This flow could possibly cause harm to the flow to be backed-up as well as to the repository by deleting files. Use it with care and at your own risk.
- This flow has only been tested with commits to the master/main branch. More sophisticated functions of Git are not supported.
