

# Backup with PG_DUMP
- Create build : `oc new-build https://github.com/jdauphant/postgresql-openshift-docker --context-dir=pg-dump-backup --name pg-dump-backup` (you may need to setup limit and request ressources if your cluster enforce it)
- Launch backup `oc run backup-db --image=pg-dump-backup --replicas=1 --restart=None`
- Create backup job and run it one : `oc create -f job-run-once.yaml`
- Relaunch the create job : `oc scale job pg-dump-backup --replicas 1`


# Command
- Launch debug shell : `oc run -i -t "pg-bash-$(date +%s)"  --restart=Never --image=centos/postgresql-10-centos7 --limits='cpu=100m,memory=256Mi' --requests='cpu=100m,memory=256Mi' --command -- "/bin/bash"`
- Restore a custom-dump (with data removal) : `pg_restore --if-exist --clean -d postgres://user:pass@host:5432/db db-dump`