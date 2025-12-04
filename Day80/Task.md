\# \*\*Day 80 — Jenkins Chained Builds | KodeKloud 100 Days of DevOps\*\*



\## 📌 \*\*Task Overview\*\*



This task demonstrates \*\*Jenkins Chained Builds\*\*, connecting multiple jobs to run sequentially:



\* \*\*Upstream Job\*\* → `Nautilus-app-deployment` (pulls code from Git repository)

\* \*\*Downstream Job\*\* → `manage-services` (restarts Apache on all App Servers)



The goal is to set up a \*\*Continuous Deployment (CD) pipeline\*\* that:



1\. Updates code on a shared storage server

2\. Restarts Apache on all App Servers \*\*only if the upstream job is successful\*\*



---



\## 🔄 \*\*Concepts\*\*



\* \*\*Chained Builds:\*\* Multiple Jenkins jobs connected so they execute \*\*in sequence\*\*.

&nbsp; Example: `JobA → JobB → JobC`



&nbsp; \* JobA finishes → triggers JobB

&nbsp; \* JobB finishes → triggers JobC



\* \*\*Upstream Job:\*\* Runs first and triggers downstream jobs when it completes.



\* \*\*Downstream Job:\*\* Runs after the upstream job completes successfully.



---



\## 🏁 \*\*Step 1 — Access Jenkins UI\*\*



\* URL: Jenkins button / local instance

\* Username: `admin`

\* Password: `Adm!n321`



---



\## ⚙️ \*\*Step 2 — Install Required Plugin\*\*



1\. Go to \*\*Manage Jenkins → Plugins → Available Plugins\*\*

2\. Search for \*\*Publish over SSH\*\*

3\. Install the plugin and \*\*restart Jenkins\*\* after installation



---



\## 🔧 \*\*Step 3 — Configure SSH Servers\*\*



1\. Navigate to \*\*Manage Jenkins → System → SSH Servers\*\*

2\. Click \*\*Add\*\* and configure:



&nbsp;  \* \*\*Storage Server\*\* (shared code directory)

&nbsp;  \* \*\*App Servers:\*\* `stapp01`, `stapp02`, `stapp03`

3\. Click \*\*Test Configuration\*\* for each server → ensure \*\*success message\*\* appears

4\. Click \*\*Save\*\*



---



\## 🚀 \*\*Step 4 — Create Upstream Job\*\*



\### Job Name: `Nautilus-app-deployment`



1\. Jenkins → \*\*New Item → Freestyle Project\*\* → Name: `Nautilus-app-deployment`

2\. \*\*Build Step:\*\* Add \*\*Send files or execute commands over SSH\*\*



&nbsp;  \* Server Name: `ststor01` (Storage Server)

&nbsp;  \* Exec Commands:



&nbsp;    ```bash

&nbsp;    cd /var/www/html

&nbsp;    git pull origin master

&nbsp;    ```

3\. \*\*Post-build Action:\*\*



&nbsp;  \* Select \*\*Build other projects\*\*

&nbsp;  \* Project to build: `manage-services`

&nbsp;  \* Check \*\*Trigger only if build is stable\*\*

4\. Click \*\*Save\*\*



---



\## 🔧 \*\*Step 5 — Create Downstream Job\*\*



\### Job Name: `manage-services`



1\. Jenkins → \*\*New Item → Freestyle Project\*\* → Name: `manage-services`

2\. Enable \*\*The project is parameterized\*\*



&nbsp;  \* Add \*\*Password Parameter\*\* for each App Server:



&nbsp;    | Variable Name  | Default Value         |

&nbsp;    | -------------- | --------------------- |

&nbsp;    | `STAPP01\_PASS` | App Server 1 password |

&nbsp;    | `STAPP02\_PASS` | App Server 2 password |

&nbsp;    | `STAPP03\_PASS` | App Server 3 password |



---



\## 🛠️ \*\*Step 6 — Configure Build Steps in Downstream Job\*\*



1\. Add \*\*Send files or execute commands over SSH\*\* for each App Server:



&nbsp;  \* \*\*stapp01\*\*



&nbsp;    ```bash

&nbsp;    echo $STAPP01\_PASS | sudo -S systemctl restart httpd

&nbsp;    ```

&nbsp;  \* \*\*stapp02\*\*



&nbsp;    ```bash

&nbsp;    echo $STAPP02\_PASS | sudo -S systemctl restart httpd

&nbsp;    ```

&nbsp;  \* \*\*stapp03\*\*



&nbsp;    ```bash

&nbsp;    echo $STAPP03\_PASS | sudo -S systemctl restart httpd

&nbsp;    ```

2\. Click \*\*Save\*\*



---



\## ✅ \*\*Step 7 — Validate the Pipeline\*\*



1\. Trigger `Nautilus-app-deployment` manually

2\. Confirm downstream job `manage-services` runs automatically \*\*only if upstream job is stable\*\*

3\. Verify Apache restarted on all App Servers:



&nbsp;  ```bash

&nbsp;  systemctl status httpd

&nbsp;  ```

4\. Verify website loads via \*\*main URL\*\*, not subdirectory



---



\## 💡 \*\*Tips \& Notes\*\*



\* \*\*Chained Builds\*\* ensure sequential execution and reduce manual intervention

\* Use \*\*parameters\*\* instead of hardcoding passwords for security

\* Always \*\*test SSH connections\*\* before adding commands to Jenkins jobs

\* Post-build actions ensure downstream jobs run \*\*only on successful builds\*\*



---



\# \*\*End of Documentation\*\*



Your Jenkins Chained Builds setup is complete and ready for Continuous Deployment.



