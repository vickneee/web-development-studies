# Working with Git & GitHub

Please download Git for your computer from the following link:
[Git Downloads.](https://git-scm.com/downloads)

## Cloning the Project from GitHub:

Follow these steps to clone the project from GitHub:

### Create a New Folder:

Begin by creating a new empty folder for your project.

### Navigate to the Folder:

```bash
# Open your terminal and move to the folder you created:
cd folderYouCreated
```  

### Clone the Repository:

```bash
# Clone your GitHub repository into the folder:
git clone yourGitHubRepositoryLink
```

### Confirm Successful Cloning:

```bash
# Verify that the cloning process was successful:
git status
```

### Access Cloned Repository:

```bash
# Enter the directory containing the cloned repository:
cd nameOfClonedRepository
``` 

By following these steps, you'll successfully clone the project from GitHub and set it up in your local environment.

## Dependency Installation:

To streamline the installation process, follow these steps:

### For Frontend:

```bash
# Navigate to the client directory:
cd client
```

```bash
# Install dependencies:
npm i
```

### For Backend:

```bash
# Move to the api directory:
cd api
```

```bash
# Install dependencies:
npm i
```

By following these instructions, you will ensure that all necessary dependencies are installed correctly for both the frontend and backend components of the application.

## Uploading Changes to the GitHub Project:

### Follow these steps every time you update the project:

>**Note:** Always create a local backup in a new folder before pushing or pulling changes from GitHub. Work on your branch to prevent conflicts in the repository.

### Check Main Branch Status:

```bash 
# Before adding your own code, ensure the main branch is up to date:
git checkout main
git pull
```

### Switch to Your Branch:

```bash    
Return to your own branch:
git checkout yourBranchName
```

```bash    
or create one:
git checkout -b yourBranchName
```

## Check Status and Branches:

```bash    
# Verify the status of your branch and check available branches:
git status
git branch
```   

### Edit Code:

Make necessary changes in your branch.

## Commit Changes:

```bash    
# Commit all changes once:
git add .
git commit -a -m "Description of your changes"
```    

## Push Changes to Your Branch:

```bash   
# Push your changes to your branch:
git push
```       

## Setting Up Remote Repository:

```bash
# If git push gives an error, set up the remote repository:
git push --set-upstream origin yourBranchName
```

```bash
# or short version:
git push -u origin yourBranchName
``` 

## Pulling Changes from Main Branch:

```bash
# If you need to sync your branch with the main branch:
git checkout yourBranchName
git pull origin main
``` 

Resolve any conflicts, if present.

## Merging Your Branch to Main:

```bash 
# Merge your branch changes into the main branch:
git checkout main
git pull
git merge yourBranchName
git push
``` 

## Deleting Unnecessary Branches:

```bash 
# List and delete unnecessary branches:
git branch
git branch -d yourBranchName
```

```bash  
# Verify the deletion:
git branch
``` 

## Creating a New Branch:

```bash 
# If needed, create a new branch:
git checkout -b yourBranchName
``` 

By following these steps, you can effectively manage changes and collaboration on the GitHub project.
