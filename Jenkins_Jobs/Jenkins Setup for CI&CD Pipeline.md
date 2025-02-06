

### 1. **Creating an SSH Key Pair in Git Bash**

We began by creating an SSH key pair to securely authenticate with GitHub using SSH for cloning, pulling, and pushing changes in the repository.


#### 1.1 **Navigating to .ssh Directory**:

Opened Git Bash and navigated to the `.ssh` directory to store the SSH key pair:



`$ cd ~/.ssh`

#### 1.2 **Generating SSH Key Pair**:

Executed the following command to generate a new SSH key pair:



`$ ssh-keygen -t rsa -b 4096 -C "haroon.younis00@gmail.com"`

The system prompted us to specify where to save the SSH key. We chose a custom name for the key:



`Enter file in which to save the key (/c/Users/ixHar/.ssh/haroon-jenkins-2-github-sapp):`

The key pair was created with:

- Private key: `haroon-jenkins-2-github-sapp`
- Public key: `haroon-jenkins-2-github-sapp.pub`

---

### 2. **Adding SSH Key to GitHub as Deploy Key**

After generating the SSH key pair, we added the public key to the GitHub repository as a deploy key for secure communication between Jenkins and GitHub.



#### 2.1 **Viewing the Public Key**:

We displayed the contents of the public key (`haroon-jenkins-2-github-sapp.pub`) to copy it:



`$ cat ~/.ssh/haroon-jenkins-2-github-sapp.pub`

#### 2.2 **Adding SSH Key to GitHub**:

- Navigated to the repository's **Settings** on GitHub.
- Selected **Deploy Keys** and added a new deploy key.
- Pasted the copied public key into the provided field.
- Set the appropriate permissions (read/write as required).
-




## **Step 1. Create a New Job in Jenkins

1. In Jenkins, click on **New Item** and choose **Freestyle Project**.
2. Name the job, e.g., **tech501-haroon-ci-test**, and click **OK**.
3. In the job configuration screen, make sure to:
    - **Description**: Add a brief description for your job (e.g., "CI job for testing the tech501-sparta-app-cicd repo").
    - Under **Discard Old Builds**, check the option **Max # of builds to keep** and set it to 5.

## **Step 2: Set Up GitHub Repository Integration

4. In the **Source Code Management** section:
    - Select **Git** and paste your GitHub repository URL. Example:
        
    
        `https://github.com/IHaroony/tech501-sparta-app-cicd/`
        
        - **Note: **Make sure the URL ends with a slash and **does not contain `.git`**.
        -
5. **Credentials**:
    - Add SSH credentials to Jenkins for GitHub access. If not already done, click **Add** and select **Jenkins**.
    - For **Kind**, choose **Username and Private Key**.
    - Enter the following:
        - **Username**: `haroon-jenkins-2-github-sapp`
        - **Private Key**: Copy the private key from your local system (use `cat haroon-jenkins-2-github-sapp` in Git Bash to display the key) and paste it into Jenkins.
    - This should resolve the SSH credential error.

### **Step 3: Configure Branches and Build Environment

6. **Branches to Build**:
    
    - Set the branch to `main` 
7. **Build Environment**:
    
    - Check **Provide Node & npm bin/ folder to PATH**.
    - Ensure Node.js is installed, and set the version to **20**.

## **Step 4: Define Build Steps

8. In the **Build** section, under **Build Steps**, select **Execute Shell**.
9. Add the following commands:
    
 
    
    `cd app npm install npm test`
    
    - This installs dependencies and runs the tests for your project.

## **Step 5: Save and Run the Job

10. Click **Save** to save your job configuration.
11. To trigger the build, click **Build Now**. Jenkins will execute the defined steps.

## **Step 6: Set Up GitHub Webhook for Notifications

To ensure that Jenkins receives notifications from GitHub when there are code changes, you need to configure a webhook in GitHub.

12. Go to your GitHub repository's **Settings** and click on **Webhooks**.
13. Click **Add webhook** and set the following:
    - **Payload URL**: Use your Jenkins server's IP address and append `/github-webhook/` to the URL. Example:
    - 
        
        `http://52.31.15.176:8080/github-webhook/`

        
    
    - **Which events would you like to trigger this webhook?**: Select **Just the push event**.
    - Disable **SSL verification**.
    - Leave everything else as default and click **Add webhook**.

## **Step 7: Trigger the Build from GitHub

14. In your local repository, modify the **README.md** to trigger the webhook. Example:
    nano README.md 

    
    `Added a line to test the webhook.`
    
15. Stage and commit the changes:
    
  
    `git add . git commit -m "added line to README" git push`
    
16. Upon pushing to the `main` branch, Jenkins should automatically start building the project (visible in the **Build Queue**).



#done 



