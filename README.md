# mysql-backup

This image runs mysqldump to backup data using cronjob to folder `/backup` mounted from a google store bucket using *gcsfuse*.

## Usage:
### APIs and Service account
install [Google cloud SDK](https://cloud.google.com/sdk/) `gcloud` command line tool and run `gcloud init` to login and configure project settings.
Enable APIs
```bash
gcloud service-management enable compute.googleapis.com
```

Create a service account with the [roles/storage.objectAdmin](https://cloud.google.com/storage/docs/access-control/iam-roles) role and generate a credentials key:

        NAME=mysql-backup
        gcloud iam service-accounts create $NAME --display-name "$NAME"
        SERVICE_ACCOUNT=$(gcloud iam service-accounts list --filter=name:"$NAME" --format='value(email)')
        GCLOUD_PROJECT=$(gcloud config get-value project)
        gcloud projects add-iam-policy-binding $GCLOUD_PROJECT --member="serviceAccount:$SERVICE_ACCOUNT" --role="roles/storage.objectAdmin"
        gcloud iam service-accounts keys create google-application-credentials.json --iam-account=$SERVICE_ACCOUNT
        kubectl create secret generic google-application-credentials.json --from-file=google-application-credentials.json
### Storage Bucket
install the *gsutil* command line tool by executing 
```bash
gcloud components install gsutil`
```
Create a [storage bucket](https://cloud.google.com/storage/docs/creating-buckets#storage-create-bucket-gsutil):
```bash
gsutil mb gs://mybucket
```

## Parameters

### Docker Usage
