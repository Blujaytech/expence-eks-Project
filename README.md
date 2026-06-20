Step 1: Clone the Repository

First, clone the project from GitHub:

git clone https://github.com/daws-81s/expense-k8.git
cd expense-k8/

Step 2: Create Namespace

Create a dedicated namespace to isolate application resources.

kubectl apply -f namespace.yaml

👉 This creates a namespace called expense

Step 3: Deploy MySQL (Database Layer)
Navigate to MySQL directory:
cd mysql/

Create/Edit manifest file:

vim manifest.yaml

Apply the configuration:

kubectl apply -f manifest.yaml

👉 This will create:

MySQL Pod
MySQL Service
⚙️ Step 4: Deploy Backend (Application Layer)
Move to backend directory:

cd ../backend/

Edit/Create manifest:
vim manifest.yaml
Deploy backend:
kubectl apply -f manifest.yaml

👉 This connects:

Backend → MySQL
🌐 Step 5: Deploy Frontend (UI Layer)
Navigate to frontend:

cd ../frontend/
Edit/Create manifest:

vim manifest.yaml
Deploy frontend:
kubectl apply -f manifest.yaml

👉 This connects:

Frontend → Backend APIs
🔍 Step 6: Verify Deployment
Check Pods:
kubectl get pods -n expense
Check Services:
kubectl get svc -n expense
🌍 Step 7: Access the Application
Option 1: Port Forwarding (Temporary Access)
kubectl port-forward svc/frontend -n expense 8080:80

👉 Access in browser:

http://localhost:8080
Option 2: Expose via LoadBalancer (External Access)

Edit frontend service:

kubectl edit svc frontend -n expense

Change:

type: ClusterIP

➡️ to:

type: LoadBalancer

Then verify:

kubectl get svc -n expense

👉 Use the EXTERNAL-IP to access application

🔄 Step 8: Set Default Namespace

To avoid using -n expense every time:

kubectl config set-context --current --namespace=expense
Verify:
kubectl config view --minify | grep namespace

👉 Output should show:

namespace: expense
