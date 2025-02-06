Intial Setup


1. **Log into Jenkins**.
2. On the Jenkins dashboard, click **New Item**.
3. Choose **Freestyle Project** and enter a name for the project.
4. Click **OK**.

### Configure Build Settings:

1. Under the **Build Discarder** section, check **Discard old builds**.
2. Set the **Max # of builds to keep** to **5**.

### Add Build Steps:

3. In the **Build Steps** section, click **Add build step** and choose **Execute shell**.
4. In the **Command** field, enter the following command:
    
    
    
    
    `uname -a`

	1.a we also built the pipeline for date  for that in excute shell we entered in:

    `date"`

### Trigger the Build:

5. On the left side of the page, click **Build Now** to trigger the build.
6. You can view the build progress and its status under **Build History** on the left.
7. View the Console Output to see if it was success 
. ![[Pasted image 20250205113607.png]]


### Configuring Additional Builds:

8. To configure another project, go to the first project’s page and click **Configure**.
9. Scroll to the bottom and click **Add post-build action**.
10. Choose **Build other projects**, choose then one for you for me it was ***haroon-date*** then select the project you want to build.
11. After configuring, click **Save** and **Apply**.

#### To see if it had worked
1. navigate to your added project which is haroon-getdate and view console output
2. the upstream should dictate that
![[Pasted image 20250205121706.png]]






