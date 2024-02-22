# Working with Git & GitHub

Git is a distributed version control system that allows developers to track changes in their codebase, collaborate with team members, and manage project versions efficiently. GitHub is a popular platform built on top of Git, providing hosting for Git repositories along with additional collaboration features.

**Key Concepts:**

1. **Repositories:** Git repositories store your project's files and their revision history. Each repository can be either local or hosted on GitHub.

2. **Commits:** Commits represent snapshots of changes made to the repository. They include a commit message describing the changes and allow for easy navigation through project history.

3. **Branches:** Branches are independent lines of development within a repository. They allow for experimentation and isolation of new features or bug fixes before merging them into the main codebase.

4. **Pull Requests:** Pull requests (PRs) are proposed changes that are submitted for review and potential integration into the main branch of the repository. They facilitate collaboration and code review among team members.

5. **Merge:** Merging combines changes from one branch into another. It is typically done after code review and approval of a pull request.

**Workflow:**

1. **Clone:** Start by cloning a repository from GitHub to your local machine using the git clone command.

2. **Branch:** Create a new branch (**git checkout -b branch-name**) to work on a specific feature or bug fix.

3. **Work:** Make changes to your codebase, adding and modifying files as needed.

4. **Commit:** Once you're satisfied with your changes, stage them (**git add**) and commit them (**git commit -m "Commit message"**) to create a snapshot.

5. **Push:** Push your committed changes to GitHub using (**git push origin branch-name**).

6. **Pull Request:** On GitHub, create a pull request to merge your changes into the main branch. Provide a descriptive title and details about the changes.

7. **Review:** Collaborators review your pull request, providing feedback and suggestions for improvement.

8. **Merge:** After addressing any feedback and obtaining approval, merge your pull request into the main branch.

**Benefits:**

1. **Collaboration:** Git and GitHub enable seamless collaboration among team members, allowing multiple developers to work on the same codebase simultaneously.

2. **Version Control:** Git tracks changes to your code over time, making it easy to revert to previous versions if needed and ensuring the integrity of your project's history.

3. **Code Quality:** Pull requests and code reviews promote code quality by facilitating feedback and ensuring adherence to coding standards.

## Downloading Git:

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

## Server Setup:

To quickly set up and run both the frontend and backend servers, follow these steps:

#### For Frontend:

Navigate to the frontend directory:
```bash
cd frontend
```

Install dependencies:
```bash
npm install or yarn
```

Start the development server:
```bash
npm start or npm run dev or yarn start
```

#### For Backend:

Move to the backend directory:
```bash
cd backend
```

Install dependencies:
```bash
npm install or yarn
```

Start the backend server:
```bash
npm start or npm run dev or yarn start
```

By following these instructions, both the frontend and backend servers will be up and running in development mode, allowing you to work on your application seamlessly.

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

or create one:
```bash    
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

## Canceling or Resetting Your Last Commit in Git:

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
