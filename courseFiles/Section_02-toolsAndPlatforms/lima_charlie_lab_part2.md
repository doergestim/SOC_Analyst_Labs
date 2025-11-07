
# **Lima Charlie meets Atomic Red**

---

### **Part 2 of 2**  

>[!Tip]
>
> _Please complete [Part 1](./lima_charlie_lab_part1.md) of this lab first._

---

In this lab we will be creating a **controlled and fake cyber attack** with **Atomic Red**.  
We will then use **Lima Charlie** to observe what is logged and how a real-world attack may trigger alerts for investigation.

---

### **Step 1: Access Lima Charlie**

Continue where we left off in Part 1, logged into the **Lima Charlie** web interface.

### **Step 2: Install the Atomic Red Plugin**

Go to the **"Add-ons"** tab in the top right of the web page:

![](attachments/ADDONS.PNG)

Scroll to find the **Atomic Red plugin** and click on **"ext-atomic-red-team"**:

![](attachments/AR.PNG)

Take a moment to explore other plugins and features Lima Charlie offers.

Click **"Subscribe"** on the right-hand side to install the plugin:

![](attachments/SUBSCRIBE.PNG)

---

### **Step 3: Run Atomic Red Tests**

After installation, return to your organization and select your machine:

![](attachments/navtoorganizations.png)  
![](attachments/selectorganization.png)

In the left menu, expand **"Extensions"** and click on **"Atomic Red Team"**:

![](attachments/extensions.png)

From the top dropdown menu, select **your device**:

![](attachments/selectdevice.png)

Scroll down to find the **command-and-control** category.  
Check the box next to the category header to select all sub-tests.

Click **"Run Tests"**:

![](attachments/C2ALL.PNG)

---

### **Step 4: Analyze the Logs**

Switch to the **"Detections"** tab on the left:

![](attachments/detections.png)  
![](attachments/logsscreen.png)

You'll see many events. Each time the page refreshes, new attacks may appear.

Look for **cmd.exe** or **powershell** executions.  
These are often indicators of **potential malicious activity** and warrant further investigation.

![](attachments/DETECTED.PNG)

---

### **Conclusion**

**Lima Charlie** proves to be a powerful and versatile tool:  
- Simple and intuitive interface  
- Detailed forensic capabilities before, during, and after an attack  
- Scalable for both small and large organizations  
- Plugin system for extended automation and detection


---
[Back to the Section](/courseFiles/Section_02-toolsAndPlatforms/toolsAndPlatforms.md)
