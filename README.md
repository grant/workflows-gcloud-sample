# workflows-gcloud-sample

https://github.com/google-github-actions/setup-gcloud

## Setup

```
gh secret set GCP_PROJECT_ID
gh secret set GCP_SA_KEY
```

```sh
# Add secret for project
PROJECT=$(gcloud config get-value project)
gh secret set GCP_PROJECT_ID -b $PROJECT

# Create service account
SERVICE_ACCOUNT=my-service-account
gcloud iam service-accounts create $SERVICE_ACCOUNT
gcloud iam service-accounts add-iam-policy-binding \
  $SERVICE_ACCOUNT@$PROJECT.iam.gserviceaccount.com \
  --member allUsers \
  --role="roles/iam.serviceAccountUser"

# Create service account key
gcloud iam service-accounts keys create sa.json \
  --iam-account=sa-name@$PROJECT.iam.gserviceaccount.com
gh secret set GCP_SA_KEY < sa.json

# Delete local service account key
rm sa.json
```