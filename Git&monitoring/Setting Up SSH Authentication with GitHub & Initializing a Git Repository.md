
## **Setting Up SSH Authentication with GitHub & Initializing a Git Repository**

### **Step 1: Generate an SSH Key**

To create a new SSH key, you used the following command:

t

`ssh-keygen -t rsa -b 4096 -C "haroon.younis00@gmail.com"`

- `-t rsa`: Specifies the type of key to generate (RSA).
- `-b 4096`: Sets the key length to 4096 bits for better security.
- `-C "your_email"`: Adds a comment (your email) to help identify the key.

After running this command, it prompted you to choose a file location. If you pressed **Enter**, it saved the key in `~/.ssh/id_rsa` (default location).


![[github1.png]]

### **Step 2: View & Copy the Public Key**

Once the SSH key was generated, you listed the `.ssh` directory contents:

`ls ~/.ssh`

Then, you displayed the public key to copy it:



`cat ~/.ssh/haroon-github.pub`

You manually copied this key to use in GitHub.

### **Step 3: Add the SSH Key to GitHub**

1. Logged into **GitHub**.
2. Went to **Settings** > **SSH and GPG keys**.
3. Clicked **New SSH Key**.
4. Entered the name of the haroon-github
5. Pasted the copied public key.
6. Saved the key.

![[github2.png]]





![[github3.png]]

### **Step 4: Start the SSH Agent & Add Key**

To use the SSH key without needing to enter the passphrase every time, you started the SSH agent:



`` eval `ssh-agent -s` ``

Then, added the SSH key:



`ssh-add ~/.ssh/haroon-github`

### **Step 5: Test SSH Connection to GitHub**

To verify that the SSH connection to GitHub was working:



`ssh -T git@github.com`

If successful, GitHub responded with:


`Hi IHaroony! You've successfully authenticated, but GitHub does not provide shell access.`

### **Step 6: Create a New Git Repository**

Navigated to the desired directory:

`cd ~/OneDrive/Documents/github/ mkdir tech501-test-ssh cd tech501-test-ssh`

Initialized the Git repository:


`git init`

Created a **README.md** file:


`echo "# tech501-test-ssh" > README.md`

Confirmed its contents:


`cat README.md`

### **Step 7: Link Git Repository to GitHub**

7. Set the branch to **main**:


`git branch -M main`

8. Added the GitHub remote repository:


`git remote add origin git@github.com:IHaroony/tech501-test-ssh.git`

### **Step 8: Add & Commit Changes**

Staged all files:


`git add .`

Committed the changes:

t

`git commit -m "added read me with one line"`

### **Step 9: Push Changes to GitHub**

Finally, you pushed the repository to GitHub:


`git push -u origin main`



Changes all done hurray