
## **1. Setting Up a CPU Usage Alert in Azure**

### **Step 1: Login to Azure Portal**

1. Open [Azure Portal](https://portal.azure.com) and log in.

### **Step 2: Navigate to Azure Monitor**

2. In the **left-hand sidebar**, select **Monitor**.
3. Click on **Alerts** in the left-hand menu.

![alerts](images/c2.png)
![alert click](c8.png)

### **Step 3: Create a New Alert**

4. Click **+ New Alert Rule** at the top.
5. Under **Scope**, click **Select a Resource** and choose your **VM**.
![VM](c1.png)

### **Step 4: Set the Condition (CPU Usage Threshold)**

![alt text](c3.png)

6. Click **Add Condition**.
7. Search for **Percentage CPU** and select it.
8. Configure:
    - **Operator:** Greater than
    - **Threshold:** **60%** (or lower for easier triggering)
    - **Time Aggregation:** **Average per minute**

![alert](c4.png)

### **Step 5: Configure Action (Email Notification)**

9. Click **+ Add Action Group**.
10. Create an action group with:
    - **Action Type:** Email
    - **Recipient Email:** Your email address

![email](c5.png)


### **Step 6: Define Alert Details & Create**

11. **Alert Rule Name:** `tech501haroon-cpu-threshold`
12. **Severity:** **2 - Warning**
13. Click **Create** to deploy the alert.

---
![aler rule](c6.png)
## **2. Load Testing the VM with Apache Bench**

To generate high CPU usage and trigger the alert, I used **Apache Bench (`ab`)**:

### **Installing Apache Bench**

Run the following command to install the tool:


`sudo apt-get install apache2-utils`

### **Running Load Test**

I executed the following command to simulate 1,000 requests with 100 concurrent users:



`ab -n 1000 -c 100 http://20.77.26.216
/`


### **Results**

- CPU usage spiked above the set threshold.
- The Azure alert was triggered.
- I received an **email notification** about high CPU usage.
-

---

## **3. Alert Triggered - Screenshot**

The following screenshot shows the alert details:

- **Alert Name:** `tech501haroon-cpu-threshold`
- **Fired Time:** **Feb 4, 2025, 5:59 PM**
- **Severity:** **Warning**
- **Affected Resource:** `tech501-haroon-3-subnet-sparta-app-vm`
- **CPU Usage Recorded:** **57.38%**

---

## **4. Cleanup - Removing the Alert and Action Group**

Once testing is complete, clean up resources:

### **Delete the Alert Rule**

14. Go to **Azure Monitor → Alerts**.
15. Find the alert **tech501haroon-cpu-threshold** and delete it.

### **Delete the Action Group**

16. Navigate to **Azure Monitor → Alerts → Action Groups**.
17. Select the action group and delete it.

### **Delete the Dashboard (Optional)**

18. Go to **Azure Portal → Dashboard**.
19. Locate and delete the monitoring dashboard if created.