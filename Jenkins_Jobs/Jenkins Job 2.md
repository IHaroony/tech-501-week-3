

## **Jenkins Job 2: Haroon-job2-ci-merge

This job automatically merges changes from the `dev` branch into the `main` branch after successful tests in **Job 1**. The job ensures a fast-forward merge, avoiding unnecessary commits and maintaining a clean history.

# **Steps:


Some of the steps to create are similar to job 1 so set will be similar few different things 
 
### **Create and Configure the `dev` Branch**

Before setting up **Job 2** in Jenkins, the `dev` branch needs to be created to allow for feature development and testing. Here are the steps to create and configure the `dev` branch:

1. **Create the `dev` Branch**:
    
    - Run the following command to create and switch to the `dev` branch:
        
        
        `git checkout -b dev`
        
2. **Make a Test Change**:
    
    - Create a simple test change by modifying a file, for example, `testfile.txt`:
        
    
        
        `echo "New change for testing Job 2" >> testfile.txt`
        
3. **Commit the Change**:
    
    - Add the modified file to the staging area:
        
     
        
        `git add .`
        
    - Commit the changes:
        
    
        
        `git commit -m "Test change in dev branch to include for Job 2"`
        
4. **Push the `dev` Branch to GitHub**:
    
    - Push the `dev` branch to the GitHub repository:
        
        
        `git push origin dev`

1. **Go to Jenkins Dashboard** → **Click New Item**.
2. Select **Freestyle project** → Name it `Haroon-job2-ci-merge` → Click **OK**.


#### **Source Code Management**:

- **Select Git**.
- Under **Repository URL**, add your GitHub repository URL:
    
    
    
    `https://github.com/yourusername/sparta-test-app-cicd/
    remember delete git and include /
    
- Under **Branches to build**, set the branch to:
    
 
    
    `dev`
    

#### **Build Steps**:

1. **Add Execute Shell**:
    - Under **Build Steps**, select **Execute Shell**.
    - Add the following script to handle the merging:
        
        
        
        `git checkout main git pull origin main git merge --ff-only origin/dev git push origin main`
        
        This script performs the following actions:
        - `git checkout main`: Switches to the `main` branch.
        - `git pull origin main`: Pulls the latest changes from the `main` branch to ensure it's up to date.
        - `git merge --ff-only origin/dev`: Merges the `dev` branch into `main` only if it’s a fast-forward merge (i.e., there are no conflicts or divergent commits).
        - `git push origin main`: Pushes the merged `main` branch back to GitHub.

#### **Post-build Actions**:

2. **Add Build other projects**:
    - Select **Haroon-job2-ci-merge**.
    - Configure it to **build only if Job 1 is successful**. This ensures that Job 2 will only run if **Job 1** (CI testing) is successful, enforcing a continuous integration workflow.


#### **SSH Agent Configuration**:

- To ensure the proper SSH key is used for Git operations (such as pushing changes), enable the **SSH Agent**:
    - In the **Build Environment** section, check **Use SSH Agent**.
    - Select the relevant SSH key (e.g., `haroon-jenkins` or the key used for GitHub access).
    - 

This configuration ensures that the Jenkins job can authenticate securely with GitHub when performing Git operations, especially when pushing to the repository.

#### **Save the Job**:

- After completing all the steps, click **Save** to create the job.

---

### **Summary of the Workflow**

- **Job 1 (Haroon-job1-ci-test)** will run the tests whenever code is pushed to the repository.
- If **Job 1** passes successfully, **Job 2 (Haroon-job2-ci-merge)** will merge the `dev` branch into `main`, ensuring that the latest changes from `dev` are reflected in `main`.
- The merge uses the `--ff-only` flag to ensure a clean fast-forward merge, avoiding unnecessary merge commits.
- The **SSH Agent** is used to authenticate and authorize Git operations securely, such as pushing the changes back to GitHub.

This setup maintains a streamlined and automated workflow for merging and deploying code change




