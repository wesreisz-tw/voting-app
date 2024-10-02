 az login
 az extension add --name containerapp --upgrade
 az provider register --namespace Microsoft.App
 az provider register --namespace Microsoft.ServiceLinker
 ENVIRONMENT=dev1-tc5
 LOCATION="eastus"
 az containerapp env create  --location "$LOCATION"  --resource-group "$RESOURCE_GROUP"  --name "$ENVIRONMENT"
 az containerapp add-on redis create --name reisz-redis-dev1-tc5-v3  --resource-group "$RESOURCE_GROUP"  --environment "$ENVIRONMENT"

 az containerapp create \\n  --name myapp \\n  --image mcr.microsoft.com/k8se/samples/sample-service-redis:latest \\n  --ingress external \\n  --target-port 8080 \\n  --bind myredis \\n  --environment "$ENVIRONMENT" \\n  --resource-group "$RESOURCE_GROUP" \\n  --query properties.configuration.ingress.fqdn

 There is an issue with bind, so I removed it and added it via the UI. Additionally, bind uses:
`
REDIS_ENDPOINT=myredis:6379
REDIS_HOST=myredis
REDIS_PASSWORD=...
REDIS_PORT=6379
`
So I rewrote a new version that used those environmental variables rather than the previous ones, so bind would automatically inject my values. This is under an azure branch.
I pushed it as v3 to dockerhub.

