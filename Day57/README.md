# Print Environment Variables in a Kubernetes Pod  
### Task: Configure and Test Environment Variables Inside a Pod

The Nautilus DevOps team is preparing the environment for an application that will send greeting messages to users.  
To validate the setup, a sample Pod must be created that prints a greeting message using environment variables.

---

## 📌 Task Requirements

1. Create a Pod named **print-envars-greeting**  
2. Container name: **print-env-container**  
3. Use image: **bash**  
4. Create the following environment variables:
   - **GREETING = Welcome to**
   - **COMPANY = Nautilus**
   - **GROUP = Group**
5. Use the command exactly as below:

["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']

6. Set `restartPolicy: Never`
7. Verify output using:

```sh
kubectl logs -f print-envars-greeting


🚀 Step 1: Create the Pod YAML
Create a file named print-envars-greeting.yaml:

apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
spec:
  containers:
    - name: print-env-container
      image: bash
      command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
      env:
        - name: GREETING
          value: "Welcome to"
        - name: COMPANY
          value: "Nautilus"
        - name: GROUP
          value: "Group"
  restartPolicy: Never


🚀 Step 2: Apply the YAML
kubectl get pods


🚀 Step 3: Verify Pod Status
kubectl get pods

Expected: Status = Completed


🚀 Step 4: View the Logs
kubectl logs -f print-envars-greeting


✅ Expected Output
Welcome to Nautilus Group


📘 Concepts Learned
Environment variables in Pods

Command override using command:

restartPolicy: Never to avoid CrashLoopBackOff

Using kubectl logs to view container output


✔️ Verification Checklist

| Checkpoint     | Command                                 | Expected Result                      |
| -------------- | --------------------------------------- | ------------------------------------ |
| Pod created    | `kubectl get pods`                      | Pod in **Completed** state           |
| Logs checked   | `kubectl logs -f print-envars-greeting` | Prints **Welcome to Nautilus Group** |
| Restart policy | YAML                                    | `restartPolicy: Never`               |



