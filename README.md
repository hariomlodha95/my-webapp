# Automate Code Deployment Using CI/CD Pipeline (GitHub Actions)

## 🚀 Objective

The goal of this task is to set up a **CI/CD pipeline** using **GitHub Actions** so that your **Node.js web application** automatically builds, tests, and deploys.

---

## 🧰 Tools Used

* **GitHub** (Code Repository + Workflow)
* **GitHub Actions** (CI/CD Pipeline)
* **Node.js** (Sample Web App)
* **Docker** (Image Build & Push)
* **DockerHub** (Container Registry)

---

## 📂 Example Project Structure

```
my-webapp/
├── Dockerfile
├── package.json
├── index.js
├── .github/
│   └── workflows/
│       └── ci-cd.yml
└── README.md
```

---

## ⚙️ Pipeline Flow (Overview)

1. **Trigger:** On every `push` to the `main` branch
2. **CI Stage:**

   * Install dependencies
   * Run application tests
3. **Build Stage:**

   * Build Docker image
4. **CD Stage:**

   * Log in to DockerHub
   * Push Docker image automatically

---

## 📝 GitHub Actions Workflow File (.github/workflows/main.yml)

Here is the complete example pipeline:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: ["main"]
  pull_request:
    branches: ["main"]

jobs:
  build_and_push:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "18"

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

      - name: Build Docker image
        run: docker build -t mywebapp:latest .

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Push image to Docker Hub
        run: |
          docker tag mywebapp:latest ${{ secrets.DOCKERHUB_USERNAME }}/mywebapp:latest
          docker push ${{ secrets.DOCKERHUB_USERNAME }}/mywebapp:latest

```

---

## 🔐 Required GitHub Secrets

To push images to DockerHub, add the following secrets in your GitHub repository:

| Secret Name          | Description             |
| -------------------- | ----------------------- |
| `DOCKERHUB_USERNAME` | Your DockerHub username |
| `DOCKERHUB_TOKEN`    | DockerHub Access Token  |

Add secrets here: **GitHub → Repository Settings → Secrets → Actions**

---

## 🐳 Sample Dockerfile

```
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]

```

---

## ▶️ Run the App Locally

```
npm install
npm start
```

---

## 🚀 Deployment

Whenever you push code to the **main branch**, the pipeline automatically:

1. Runs tests
2. Builds the app
3. Builds a Docker image
4. Pushes the image to DockerHub

Your deployment is now fully automated.

---

## ✔️ Conclusion

By following this README, you will have a fully functional **automated CI/CD pipeline** using GitHub Actions for building and deploying a Node.js application.
