# ⭐ Celebrity Detector
A web-based application that detects celebrity faces in uploaded images using OpenCV and identifies them via a powerful LLM (LLaMA-4 Maverick 17B). 
Users can explore detailed information about the recognized celebrity and ask follow-up questions all-in-one seamless interface.

---

## 🔧 Core Workflow
1. **🖥️ Application**: Built with Flask (backend) and HTML/CSS (frontend).
2. **📸 Upload Image**: User uploads an image via the web interface.
3. **👁️ Face Detection**: OpenCV scans the image using Haar Cascade to detect frontal faces.
4. **🧠 LLM Processing**: Detected face is passed to the LLM 🦙 `llama-4-maverick-17b-128e-instruct` for identification.
5. **📜 LLM Response**: Once identified, the app displays details about the celebrity.
6. **🗣️ Interaction**: User can ask any question (e.g., family, marriage, etc) and LLM generates contextual answers based on the celebrity's profile.
7. **🐳 Deployment**: Deploying on a GKE cluster & using GAR for storing docker images. 
8. **🌐 Github**: Github servers as SCM & code version for CircleCI to automate CI/CD workflow.

---

## 📦 Installating & Running Locally
- **Clone this repo & CD Celebrity-Detector :**
  ```
  git clone https://github.com/P-RajaRamesh/Celebrity-Detector.git
  ```
- **Create Virtual Environment :** ```conda create -p venv1 python==3.10```
- **Activate Virtual Environment :** ```conda activate venv1\```
- **Install Requirements :** ```pip install -e .```
- **Create ```.env``` file:** ```GROQ_API_KEY="<YOUR-GROQ-API-KEY>"```
- Finally Run Flask app: ```python app.py```

---

## 💻 Pushing code to your Github
```
git init
git add .
git commit -m "commit"
git branch -m main
git remote add origin https://github.com/P-RajaRamesh/Celebrity-Detector.git
git push origin main
```
---

## ☁️ Deploying in Google Kubernetes Engine (GKE):
### Step 1. ⏪ Pre-Deployment Checklist
Check that you have done the following before moving to the CircleCI deployment part:
1. ✅ Dockerfile  
2. ✅ Kubernetes Deployment file  
3. ✅ Code Versioning using GitHub

---

### Step 2. ✓ Enable Required GCP APIs
Go to your GCP account and enable all the following APIs:
**Navigation:** In the left pane -> **APIs & Services** -> Click ➕ **Enable APIs and services** <br/>
Enable the following:
- Kubernetes Engine API  
- Container Registry API  
- Compute Engine API  
- Cloud Build API  
- Cloud Storage API  
- IAM API

---

### Step 3. 🛠️ Create GKE Cluster 💠 and Artifact Registry 📒
1. **💠 Create GKE Cluster:**
   - Go to your GCP Console and search for **Kubernetes Engine** -> **Clusters**.
   - Create a 🆕 cluster with any name: `celebrity-cluster`
   - In the **Networking** section, check
     - ☑ **Access using DNS**
     - ☑ **Access using IPv4 addresses**
     - ◻ ❌ Don't check **Enable authorized networks**
   - Click **Create**

2. **📒 Create Artifact Registry:**
   - In the GCP Console, search for **Artifact Registry**.
   - Create a 🆕 Artifact Registry with a name (`llmops-repo`).

---

### Step 4. 👨‍🔧 Create a Service Account and Configure Access
1. Goto **IAM & Admin** -> **Service accounts** -> ➕ **Create a Service Account**
2. Enter any service name and **Assign the following roles:**
   - ✅ Storage Object Admin  
   - ✅ Storage Object Viewer  
   - ✅ Owner  
   - ✅ Artifact Registry Admin  
   - ✅ Artifact Registry Writer
3. Click **Done**
4. Come back to your servive account and click on ⋮ **Manage Keys** -> **Add Key** -> **Create Key**
5. 💾 **Download the key** as a `.json` file.
6. Rename the key `gcp-key.json` and place in your **root directory** of your project.
7. ❗ **Important:** Add `gcp-key.json` to your `.gitignore` to prevent it from being pushed to GitHub.

---

### Step 5. 🔐 Convert `gcp-key.json` to Base64
Open git bash terminal in VSCode and browse to your project directory, run:
```bash
cat gcp-key.json | base64 -w 0
```
Copy the output and use it in environment variables for CircleCI secrets.

---

### Step 6. 🛠️ Set Up CircleCI Configuration file
1. **Create the CircleCI config file:** <br/>
   In your project root directory, create the following file: ```.circleci/config.yml```
2. **Get the code** for config file from my GitHub repository: `https://github.com/P-RajaRamesh/Celebrity-Detector.git`
3. **Set up a CircleCI account:**
   - After creating the account, connect it to **GitHub** to access your repositories.

---

### Step 7. 🤝 Connect Project to CircleCI and Set Environment Variables
1. **Open CircleCI** and go to the **Projects** section.
   - **Create Project** -> **Build, test & deply your software application**
   - Give any project name: `LLMOPS`
   - Click **Setup a pipeline**
   - Give any pipeline name `build-and-test`
   - Next you need to Choose the Github Repo
3. **Connect CircleCI to your GitHub account:**
   - Authorize CircleCI to access your GitHub repositories.
4. **Select your project repository**:
   - CircleCI will automatically detect the `.circleci/config.yml` file.
5. **Configure project settings:**
   - Set up **triggers** for **When would you like to run your pipeline?** -> **All pushes**
6. Come back to you project created `LLMOPS`
   - Click on ··· -> Select **Project settings** -> **Environment Variables** section
5. **Add the following environment variables**:
   - ✅`GCLOUD_SERVICE_KEY` — your Base64-encoded GCP key which you have copied in **Step 5**.
   - ✅`GOOGLE_PROJECT_ID` — your GCP project ID  
   - ✅`GKE_CLUSTER` — your GKE cluster name `celebrity-cluster` 
   - ✅`GOOGLE_COMPUTE_REGION` — your compute region

---

### Step 8. 🗝️ Set Up LLMOps Secrets in GKE using kubectl
- Click your cluster `celebrity-cluster` -> open terminal.
- Run the below command to connect with your cluster if not already ran automatically
```
gcloud container clusters get-credentials <YOUR-CLUSTER-NAME> \
--region us-central1 \
--project <YOUR-PROJECT-ID>
```
- Run the below command to inject the **GROQ_API_KEY** secrets
```
kubectl create secret generic llmops-secrets \
--from-literal=GROQ_API_KEY="<YOUR-API-KEY>"
```
---

### Step 9. ⚡Trigger CircleCI Pipeline
- Goto CIrcleCI -> CLick your Project `LLMOPS` -> Select **Pipeline** section
- Filter with you `LLMOPS` project & `main` branch
- Click **Trigger Pipeline** on top right corner.
- From now on, the pipeline will be **automatically triggered** on each `git push` to the repository.

---

### Step 10. 👨‍💻 Access the application
- Goto **Kubernetes Engine** -> **Workloads**
- Click `llmops-app` and scroll down to see the **Exposing services**.
- Click **External endpoints** link ↗ and access you application.

---

---
