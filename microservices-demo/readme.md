d:\helm\microservices-demo>helm template microservices-demo .

d:\helm\microservices-demo>helm upgrade microservices-demo . --install --namespace microservices-demo


helm list -n microservices-demo


helm history microservices-demo -n microservices-demo

helm rollback microservices-demo 1 --namespace microservices-demo
helm rollback microservices-demo --namespace microservices-demo

Parent value always override subchart value!


Folder Structure MUST Match Values Key:
text
microservices-demo/
├── Chart.yaml
├── values.yaml
└── charts/
    ├── user-service/          # ← Subchart folder name
    │   ├── Chart.yaml
    │   └── templates/
    └── order-service/         # ← Subchart folder name  
        ├── Chart.yaml
        └── templates/

Parent values.yaml MUST Use Same Names:
yaml
# These keys MUST match the subchart folder names
user-service:          # ← Matches "user-service/" folder
  replicas: 2
  image:
    repository: myrepo/user-service

order-service:         # ← Matches "order-service/" folder  
  replicas: 3
  image:
    repository: myrepo/order-service




How Values Flow with Dependencies:
With Dependencies Defined:
text
Parent values.yaml
       ↓
Merges with subchart values.yaml
       ↓  
Final values used in subchart templates
Without Dependencies:
text
Parent values.yaml
       ↓
ONLY parent values used
       ↓
Subchart values.yaml IGNORED

This command check what exact values will be deployed:
helm get values microservices-demo -n microservices-demo --all
 
*** sometimes -set parameter override the values in values.yaml. Need use --reset-valuse to reset it

helm upgrade microservices-demo . \
  -n microservices-demo \
  --reset-values
