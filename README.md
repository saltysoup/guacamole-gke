# Guacamole on GKE
This is to deploy Guacamole stack (guacamole, guacd and relational DB) on Google Cloud using Kubernetes.

## Instructions

1. Create a new GKE cluster and configure local kubectl client to it
1. Deploy the stack using kubectl apply -f .

### DB Initialization
1. Copy the local DB initialization scripts to the db container 
1. kubectl cp -n netboot-guacamole 001-create-schema.sql guacamole-db-stateful-0:/tmp/
1. kubectl cp -n netboot-guacamole 002-create-admin-user.sql guacamole-db-stateful-0:/tmp/
1. Shell into the db container
1. kubectl exec -it -n netboot-guacamole guacamole-db-stateful-0 -- /bin/bash
1. Enter mysql using credentials (available in secret.yml and passed as env var)
1. mysql -u root -p (guacadmin when prompted)
1. use guacamole;
1. source /tmp/001-create-schema.sql
1. source /tmp/002-create-admin-user.sql
1. Exit the shell from db container
1. Use your browser to go to the ingress IP access
1. When prompted, default login is guacadmin//guacadmin

## TODO:
1. Look at Cloud SQL as alternative platform to run mysql instead of containerized statefulset pod
1. Change ingress type to use internal load balancer annotation and import SSL certificate (Google Managed SSL cert not available yet)
1. Integrate LDAP using Microsoft AD and one way forest trust to extend with existing AD environment
1. Enable 2FA extension on Guacamole
1. Automatic VM discovery using Stack Driver, Cloud Functions and mysql DB
1. Enable Session Recording and [mount GCS bucket as volume](https://cloud.google.com/storage/docs/gcs-fuse) in guacamole pod (should be ok because files are pushed one way into bucket, with no IO streaming pipe open)