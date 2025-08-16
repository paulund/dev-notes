# GCP VM Port Forwarding

Use Google Cloud Identity-Aware Proxy (IAP) to create secure SSH tunnels to your VM instances for port forwarding.

## Basic Port Forwarding Command

```bash
gcloud compute ssh --zone "{REGION}" "{VM-NAME}" --tunnel-through-iap --project "{PROJECT-ID}" -- -L 9030:{IP-ADDRESS}:3306
```

## Parameters Explained

- `{REGION}`: The zone where your VM is located (e.g., `us-central1-a`)
- `{VM-NAME}`: The name of your Compute Engine instance
- `{PROJECT-ID}`: Your Google Cloud project ID
- `9030`: Local port on your machine
- `{IP-ADDRESS}`: Internal IP of the target service (often `localhost` or `127.0.0.1`)
- `3306`: Remote port (MySQL port in this example)

## Common Use Cases

**Database Access:**
```bash
# MySQL/MariaDB
gcloud compute ssh --zone "us-central1-a" "my-vm" --tunnel-through-iap --project "my-project" -- -L 3306:localhost:3306

# PostgreSQL
gcloud compute ssh --zone "us-central1-a" "my-vm" --tunnel-through-iap --project "my-project" -- -L 5432:localhost:5432
```

**Web Services:**
```bash
# Access web application on port 8080
gcloud compute ssh --zone "us-central1-a" "my-vm" --tunnel-through-iap --project "my-project" -- -L 8080:localhost:8080
```

## Prerequisites

1. Enable IAP TCP forwarding in your GCP project
2. Ensure your VM has the IAP tunnel access firewall rule
3. Have the appropriate IAM permissions (`roles/iap.tunnelResourceAccessor`)
