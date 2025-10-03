
# **LimaCharlie Lab**

---

## **Part 1 of 2 – Endpoint Monitoring & Threat Detection**

In this lab we will be using **LimaCharlie** to investigate **endpoint monitoring** and **threat detection**.

### **What is LimaCharlie?**

LimaCharlie is a lightweight browser-based tool. It helps monitor systems, detect threats, and respond quickly to suspicious activity. This walkthrough uses the **Chrome** browser, but any browser may be used.

---

## **Step 1: Access LimaCharlie**

Open a browser and go to:

```
https://app.limacharlie.io/
```

Click **"Create an account"**.

![](attachments/register_an_account.PNG)

---

## **Step 2: Sign Up**

Choose a sign-up method:

![](attachments/LimaCharlie_signupmethod.png)

Select **"Sign up with email"**:

![](attachments/SIGN_UP_BUTTON.PNG)

Fill out the required fields and click **"Sign Up"**.

Check your email, click the verification link, then return to your browser and refresh the page.

---

## **Step 3: Setup Company Information**

You will be asked questions about your company. Use fictional information.

Enter the following details:

- **What best describes your team/company?** → *Security Operations Center*
- **What best describes your role?** → *Security Engineer*
- **What use cases are you exploring?** → *Endpoint Detection & Response*
- **How did you hear about us?** → *Black Hills Info Sec*

![](attachments/company_setup_menu.PNG)

Check the box to agree to the Terms of Service and Privacy Policy.

Click **"Get Started"** then **"Create Organization"**.

![](attachments/create_an_organization.PNG)

Fill out your fictional organization’s details.

![](attachments/ficticious_company_selection.PNG)

Click **"Create Organization"**.

Wait for the organization to be created.

![](attachments/selectorganization.png)

Select your organization to continue.

---

## **Step 4: Create a Sensor**

Under the left-hand menu, go to **Sensors → Installation Keys**.

![](attachments/one.PNG)

Click **"Create Installation Key"**.

![](attachments/two.PNG)

Fill in a description and tags, then click **"Create"**.

![](attachments/three.PNG)

Next, go to **Sensors List**.

![](attachments/four.PNG)

Click **"Add Sensor"**.

![](attachments/addsensor.png)

Scroll down and select the **Windows** sensor.

![](attachments/five.PNG)

From the drop-down, select the installation key created earlier and click **"Select"**.

![](attachments/six.PNG)

Choose the architecture: **"86-64 exe"**.

![](attachments/seven.PNG)

Click **"Download the selected installer"**.

Copy the command string from **Step 4**.

![](attachments/eight.PNG)

---

## **Step 5: Install the Sensor**

Go to your desktop, right-click **Windows Terminal**, and select **"Run as administrator"**.

![](attachments/nine.PNG)

Navigate to your Downloads directory:

```
cd .\Downloads
```

Begin typing the following command and use **Tab** for auto-completion:

```
.\hcp_win_x64_release_4.29.2.exe
```

Paste the copied command string and press **Enter**.

If successful, your output will look like this:

![](attachments/correctoutput.png)

Return to your browser. You should see a confirmation message:

![](attachments/success.PNG)

>[!TIP]
>
> **Note**: Your computer name will differ.

---

## **Next Steps**

- [Continue to Part 2 of the Lab](./lima_charlie_lab_part2.md)


>[!IMPORTANT]
>
> **Important**: Always destroy your lab environment after completing exercises.

---
[Back to the Section](/courseFiles/Section_02-toolsAndPlatforms/toolsAndPlatforms.md)
