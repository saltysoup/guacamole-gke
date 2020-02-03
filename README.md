# Guacamole on GKE
This is to deploy


## Instructions

1. Create a new GKE cluster and configure local kubectl client to it
1. Deploy the stack using kubectl apply -f .
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