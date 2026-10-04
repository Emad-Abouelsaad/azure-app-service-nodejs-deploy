# Node.js REST API on Azure App Service (CI/CD with GitHub Actions)

Cloud computing lab: deploying a Node.js / Express REST API to **Azure App Service** with an automated **GitHub Actions** build-and-deploy pipeline.

## What this project demonstrates
- **Azure App Service**: created and configured a Linux Web App (Node.js 20) and deployed the API to it.
- **CI/CD**: GitHub Actions workflow with two jobs — *build* (install, build, test, upload artifact) and *deploy* (download artifact, deploy to the Web App).
- **Secure authentication**: the pipeline signs in to Azure with **OpenID Connect (OIDC)** federated credentials — no passwords or publish profiles stored in the repository; client, tenant and subscription IDs are kept as GitHub secrets.
- **REST API**: Express endpoints for accounts and transactions (create, read, delete) with input validation and proper HTTP status codes (201, 400, 404, 409).

## API endpoints
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Health check ("Hello World!") |
| POST | `/api/accounts` | Create an account |
| GET | `/api/accounts/:user` | Get account details and transactions |
| DELETE | `/api/accounts/:user` | Delete an account |
| POST | `/api/accounts/:user/transactions` | Add a transaction (updates balance) |
| DELETE | `/api/accounts/:user/transactions/:id` | Delete a transaction |

## Tech stack
Node.js, Express, Microsoft Azure (App Service), GitHub Actions, OIDC

## Run locally
```bash
npm install
npm start
# open http://localhost:3000
```

## Tests
```bash
npm test
```
Six API tests (Node.js test runner) start the server and check every endpoint, including the status codes and the
balance after adding and deleting a transaction. They run on every push in
[`.github/workflows/ci.yml`](.github/workflows/ci.yml).

## Pipeline
See [`.github/workflows/azure-webapp-deploy.yml`](.github/workflows/azure-webapp-deploy.yml).
The deployment workflow is set to **manual run** in this copy, because the Azure secrets belong to the original lab
subscription.

## Credits
The API code is based on Microsoft's open-source *App Service Hello World* sample and the *Web Dev For Beginners* bank API (MIT License — see [`LICENSE`](LICENSE) and [`MICROSOFT_SAMPLE_README.md`](MICROSOFT_SAMPLE_README.md)).
The Azure deployment setup and CI/CD pipeline were done by **Emad Abouelsaad** as part of the Cloud Computing program at WSB University.
