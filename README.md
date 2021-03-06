# workflows-gcloud-sample

Sets up CD for Cloud Workflows. Uses tech:

- gcloud
- Cloud Workflows
- A service account
- GitHub Actions
  - https://github.com/google-github-actions/setup-gcloud

## Setup

This script sets up your service account (for Workflows) with GitHub:

```sh
# Add secret for project
PROJECT=$(gcloud config get-value project)
gh secret set GCP_PROJECT_ID -b $PROJECT

# Create service account
SERVICE_ACCOUNT=my-wf-service-account44
gcloud iam service-accounts create $SERVICE_ACCOUNT
gcloud projects add-iam-policy-binding $PROJECT \
--member "serviceAccount:$SERVICE_ACCOUNT@$PROJECT.iam.gserviceaccount.com" \
--role "roles/workflows.editor"
gcloud projects add-iam-policy-binding $PROJECT \
--member "serviceAccount:$SERVICE_ACCOUNT@$PROJECT.iam.gserviceaccount.com" \
--role "roles/iam.serviceAccountUser"

# Create service account key, upload it to GitHub, then delete it locally
gcloud iam service-accounts keys create sa.json \
--iam-account=$SERVICE_ACCOUNT@$PROJECT.iam.gserviceaccount.com
gh secret set GCP_SA_KEY < sa.json
rm sa.json
```

Essentially, the following:

```
gh secret set GCP_PROJECT_ID
gh secret set GCP_SA_KEY
```