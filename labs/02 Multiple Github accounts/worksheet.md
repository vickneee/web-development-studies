# Managing Multiple GitHub Accounts with One Git Identity

If you want to use the same Git identity (username and email) across multiple GitHub accounts, you can achieve this by configuring each repository with the appropriate GitHub account's remote URL. Here's how you can do it:

1. **Configure Git locally for each repository**: Navigate to the directory of each repository and set the username and email for that repository.

    ```bash
    # Navigate to the directory of the repository
    cd /path/to/repository

    # Set user.name and user.email for this repository
    git config --local user.name "Your Name"
    git config --local user.email "your.email@example.com"
    ```

   Repeat this step for each repository associated with a different GitHub account.

2. **Set up different remote URLs**: For each repository, configure the remote URL to point to the appropriate GitHub account. You can do this by updating the `origin` remote URL.

    ```bash
    # Check the current remote URL
    git remote -v

    # Change the remote URL to point to the appropriate GitHub account
    git remote set-url origin git@github.com:username/repo.git
    ```

   Replace `username` with the username of the GitHub account you want to use for this repository, and `repo` with the name of the repository.

3. **Push and pull changes**: Now you can push and pull changes to and from each repository, and Git will use the appropriate GitHub account based on the remote URL configured for that repository.

By following these steps, you can use the same Git identity across multiple GitHub accounts and manage your repositories accordingly.
