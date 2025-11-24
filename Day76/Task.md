Jenkins Job Permission Setup — xFusionCorp Industries
Task Overview
The development team has requested that two new developers (sam and rohan) be granted permissions on the existing Jenkins job Packages.
The goal is to assign only the necessary permissions to these users while ensuring Jenkins runs without errors.

Prerequisites
	• Jenkins admin credentials:
		○ Username: admin
		○ Password: Adm!n321
	• Existing users:
		○ sam → password: sam@pass12345
		○ rohan → password: rohan@pass12345
	• Existing Jenkins job: Packages
	• Plugins: Role-based Authorization Strategy or Matrix Authorization Strategy

Step 1: Login to Jenkins
	1. Open the Jenkins UI.
	2. Login as admin using Adm!n321.

Step 2: Install Required Plugins
	1. Navigate to: Manage Jenkins → Manage Plugins → Available.
	2. Search for Role-based Authorization Strategy (or Matrix Authorization Strategy).
	3. Install the plugin and select Restart Jenkins when installation is complete and no jobs are running.

Step 3: Configure Global Security
	1. Go to Manage Jenkins → Configure Global Security.
	2. Under Authorization, select: Project-based Matrix Authorization Strategy.
	3. Add users sam and rohan.
	4. Assign overall Read permission to both users (mandatory to avoid errors).
	5. Save changes.
	⚠️ Important: Without this global Read permission, job-specific permissions will not work correctly.

Step 4: Assign Job-specific Permissions
	1. Navigate to the job: Packages → Configure → Enable Project-based Security.
	2. Check Use project-based security.
	3. Ensure Inherit permissions from parent ACL is enabled.
	4. Assign permissions for each user:
	User	Job Permissions
	sam	Build, Configure, Read
	rohan	Build, Cancel, Configure, Read, Update, Tag
	5. Save the configuration.

Step 5: Verification
	1. Login as sam:
		○ Verify the user can read, build, and configure the Packages job.
	2. Login as rohan:
		○ Verify the user can read, build, cancel, configure, update, and tag the Packages job.

Step 6: Documentation / Evidence
	• Take screenshots of:
		1. Global security showing sam and rohan with overall Read permission.
		2. Job-specific security page showing assigned permissions.
	• Optional: Record a Loom video demonstrating the configuration and verification process.

Key Notes / Best Practices
	• Only modify permissions for the specified users.
	• Always enable Inherit permissions from parent ACL for job-specific settings.
	• Global Read permission is required to avoid Jenkins errors when applying job-specific permissions.
<img width="923" height="1500" alt="image" src="https://github.com/user-attachments/assets/31aa1fc4-3fa7-4f3d-b931-f6b579fb34d6" />
