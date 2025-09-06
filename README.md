# ⭐ Celebrity Detector
A web-based application that detects celebrity faces in uploaded images using OpenCV and identifies them via a powerful LLM (LLaMA-4 Maverick 17B). 
Users can explore detailed information about the recognized celebrity and ask follow-up questions all-in-one seamless interface.

## 🔧 Core Workflow
1. **🖥️ Application**: Built with Flask (backend) and HTML/CSS (frontend).
2. **📸 Upload Image**: User uploads an image via the web interface.
3. **👁️ Face Detection**: OpenCV scans the image using Haar Cascade to detect frontal faces.
4. **🧠 LLM Processing**: Detected face is passed to the LLM 🦙 `llama-4-maverick-17b-128e-instruct` for identification.
5. **📜 LLM Response**: Once identified, the app displays details about the celebrity.
6. **🗣️ Interaction**: User can ask any question (e.g., family, marriage, etc) and LLM generates contextual answers based on the celebrity's profile.

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

## 💻 Pushing code to your Github
```
git init
git add .
git commit -m "commit"
git branch -m main
git remote add origin https://github.com/P-RajaRamesh/Celebrity-Detector.git
git push origin main
```

## ☁️ Deploying in Google Kubernetes Engine (GKE):
