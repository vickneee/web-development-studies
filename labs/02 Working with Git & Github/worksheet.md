# Working with Git & GitHub

Please download Git for your computer from the following link:
[Git Downloads.](https://git-scm.com/downloads)

## Cloning the Project from GitHub:

Follow these steps to clone the project from GitHub:

### Create a New Folder:

Begin by creating a new empty folder for your project.

### Navigate to the Folder:

Open your terminal and move to the folder you created:
```bash
cd folderYouCreated
```  

### Clone the Repository:

Clone your GitHub repository into the folder:
```bash
git clone yourGitHubRepositoryLink
```

### Confirm Successful Cloning:

Verify that the cloning process was successful:
```bash
git status
```

### Access Cloned Repository:

Enter the directory containing the cloned repository:
```bash
cd nameOfClonedRepository
``` 

By following these steps, you'll successfully clone the project from GitHub and set it up in your local environment.

## Dependency Installation:

To streamline the installation process, follow these steps:

### For Frontend:

Navigate to the frontend directory:
```bash
cd frontend
```

Install dependencies:
```bash
npm install or yarn
```

### For Backend:

Move to the backend directory:
```bash
cd backend
```

Install dependencies:
```bash
npm install or yarn 
```

By following these instructions, you will ensure that all necessary dependencies are installed correctly for both the frontend and backend components of the application.

## Starting the Projects:

After installing the dependencies, you can start both the frontend and backend projects with the following commands:

### For Frontend:

Navigate to the frontend directory:
```bash
cd frontend
```

Install dependencies:
```bash
npm start or yarn start
```

### For Backend:

Move to the backend directory:
```bash
cd backend
```

Install dependencies:
```bash
npm start or yarn start
```

By following these instructions, you'll be able to start both projects smoothly.

## Uploading Changes to the GitHub Project:

### Follow these steps every time you update the project:

>**Note:** Always create a local backup in a new folder before pushing or pulling changes from GitHub. Work on your branch to prevent conflicts in the repository.

### Check Main Branch Status:

Before adding your own code, ensure the main branch is up to date:
```bash 
git checkout main
git pull
```

### Switch to Your Branch:

Return to your own branch:
```bash    
git checkout yourBranchName
```

```bash    
or create one:
git checkout -b yourBranchName
```

## Check Status and Branches:

Verify the status of your branch and check available branches:
```bash    
git status
git branch
```   

### Edit Code:

Make necessary changes in your branch.

## Commit Changes:

Commit all changes once:
```bash    
git add .
git commit -a -m "Description of your changes"
```    

## Push Changes to Your Branch:

Push your changes to your branch:
```bash   
git push
```       

## Setting Up Remote Repository:

If git push gives an error, set up the remote repository:
```bash
git push --set-upstream origin yourBranchName
```

or short version:
```bash
git push -u origin yourBranchName
``` 

## Creating a Pull Request:

1. After pushing your changes to your branch, go to your repository on GitHub.
2. Click on the "Pull Requests" tab.
3. Click on the "New Pull Request" button.
4. Select the base branch (usually main) and the branch with your changes.
5. Review the changes and click on the "Create Pull Request" button.
6. Provide a title and description for your pull request.
7. Click on the "Create Pull Request" button to submit your pull request.

## Merging a Pull Request:

1. Once your pull request is reviewed and approved:
2. If needed, go to your pull request on GitHub.
3. Click on the "Merge Pull Request" button.
4. Confirm the merge by clicking on the "Confirm Merge" button.
5. Optionally, delete the branch after merging by clicking on the "Delete Branch" button.

## Pulling Changes to your branch from the Main Branch:

If you need to sync your branch with the main branch:
```bash
git checkout yourBranchName
git pull origin main
``` 

Resolve any conflicts, if present.

## Merging Your Branch to Main:

If needed, merge your branch changes into the main branch:
```bash 
git checkout main
git pull
git merge yourBranchName
git push
```

## Deleting Unnecessary Branches:

List and delete unnecessary branches:
```bash 
git branch
git branch -d yourBranchName
```

Verify the deletion:
```bash  
git branch
``` 

## Creating a New Branch:

If needed, create a new branch:
```bash 
git checkout -b yourBranchName
``` 

By following these steps, you can effectively manage changes and collaboration on the GitHub project.

## Canceling or Resetting Your Last Commit in Git

Sometimes, you might need to undo the last commit you made in Git. Here's how you can do it:

**Open your terminal or command prompt.**

**Navigate to your Git repository using the `cd` command.**

```bash
cd path/to/your/repository
```

1. **Check the commit history to confirm the last commit you want to cancel.**

```bash
git log
```

2. **Use the git reset command to cancel the last commit.**

```bash
git reset --hard HEAD~1
```

This command will move the HEAD pointer to the commit before the last commit (HEAD~1) and discard all changes introduced by the last commit.
The --hard option indicates that both the working directory and the index will be reset to match the specified commit.

3. **Verify that the last commit has been canceled by checking the commit history again.**

```bash
git log
```

4. **If you've pushed the changes to a remote repository, you may need to force to push the changes to update the remote repository.**

```bash
git push origin HEAD --force
```

>**Note**: Be cautious when using git push --force as it overwrites the remote repository's history. Make sure you're not affecting other collaborators' work.

5. **Once you've canceled the last commit, you can make any additional changes and commit them as needed.**

By following these steps, you can successfully cancel or reset your last commit in Git, allowing you to make further changes or corrections as necessary.

