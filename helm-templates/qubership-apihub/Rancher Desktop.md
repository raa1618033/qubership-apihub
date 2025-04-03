## Qubership APIHUB Installation on local k8s cluster (Rancher Desktop by SUSE)

Rancher Desktop Official site: https://rancherdesktop.io/
Install Rancher Desktop accordingly https://docs.rancherdesktop.io/getting-started/installation

### Ingress Controller
By default Ingress Controller in Rancher Desktop uses ports 80 and 443.
Check the listening ports are available for Ingress Controller.
If those ports are unavailable you should change Ingress Controller ports or use port-forward.

### Generate secrets
Navigate to qubership-apihub\helm-templates\qubership-apihub directory
run generate_jwt_pkey.sh to generate jwt_private_key file
run generate-local-passwords.sh to generate local-secrets.yaml  file

### Deploy postgres DB
run 
`helm install postgres-db -n postgres-db --create-namespace ..\helm-templates\postgres-db`
where ..\helm-templates\postgres-db is the path to postgres-db CHART


### Deploy qubership-apihub appliction
run
`helm install apihub -n apihub --create-namespace -f ..\helm-templates\qubership-apihub\local-k8s-values.yaml -f ..\helm-templates\qubership-apihub\local-secrets.yaml ..\helm-templates\qubership-apihub`

where ..\helm-templates\qubership-apihub is the path to qubership-apihub CHART

### check pods have status Running
`kubectl get pods -n apihub`

### uninstall  qubership-apihub
run
`helm uninstall apihub -n apihub`
`helm uninstall postgres-db -n postgres-db`