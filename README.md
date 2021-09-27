#### Creating a New Repository
  1. Use the __+__ drop down menu in the upper right corner of your github page, and select **New repository**.
  2. open command prompt and change the current working directory to your local project.
  3. Initialize the local directory as a Git repository.
     ```
     $ git init -b main
     ```
  4. create README.md file using code editor
  5. Add and Commit the file in your new local repository 
     ```
     $ $ git add README.md
     $ git commit -m "First commit"
     ```
  6. copy the remote repository URL to add and push it.
     ```
     $ git remote add origin  <REMOTE_URL> 
     $ git push origin main
     ```
