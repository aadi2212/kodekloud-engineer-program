# 📄 Jenkins Parameterized Job Documentation — `parameterized-job`

Simple parameterized build setup for testing basic Jenkins parameter functionality.

---

## 📌 Task Objective

Create a Jenkins **parameterized job** that:

- Accepts user-provided parameters  
- Runs a shell script that prints the parameter values  
- Builds successfully using the parameter value **Staging**

---

## 🚀 1. Login to Jenkins

Open Jenkins UI and login:

- **Username:** admin  
- **Password:** Adm!n321  

---

## 🧱 2. Create a New Jenkins Job

Navigate:


New Item → Freestyle Project


Enter job name:


parameterized-job


Click **OK**.

---

## ⚙️ 3. Configure Job Parameters

Check:


This project is parameterized


### 📝 String Parameter

| Field | Value |
|-------|--------|
| **Name** | Stage |
| **Default Value** | Build |
| **Description** | Enter the stage name |

### 🔽 Choice Parameter

| Field | Value |
|-------|--------|
| **Name** | env |
| **Choices** | Development<br>Staging<br>Production |
| **Description** | Environment to build/deploy |

Ensure **Staging** is spelled correctly.

---

## 🖥 4. Add Build Step – Execute Shell

Navigate:


Build → Add build step → Execute shell


Add script:

```bash
echo "Stage parameter value: $Stage"
echo "Env parameter value: $env"

Save the job.


▶️ 5. Build the Job

Click:
Build with Parameters

Select:

Stage: Build

env: Staging


Run the build.

Check Console Output:
Stage parameter value: Build
Env parameter value: Staging


❗ Errors Encountered & Fixes

1️⃣ Incorrect Choice Parameter Spelling

Issue: Entered Stage instead of Staging in env choices.
Fix: Corrected to exact choice values:

Development
Staging
Production


2️⃣ Job Name Typo

Issue: Job initially created as paramerized-job.
Jenkins returned:
job 'parameterized-job' not found


Fix: Recreated job with exact name:
parameterized-job


✅ Final Result

✔ Parameterized job created successfully

✔ String & choice parameters working

✔ Build executed with Staging choice

✔ Output correctly displayed in console


📌 Notes

Always verify job names, parameter names, and spellings

Jenkins UI may require plugin install/restart to fully enable parameter options

Use screenshots/recording to document UI actions if required


🎉 Task Completed Successfully
