Referenced from: https://agentgateway.dev/docs/kubernetes/latest/install/helm/


# The following commands are meant for testing requests through the gateway

**Port Forward to simulate request passing through MCP Gateway**
`kubectl port-forward deployment/agentgateway-proxy -n agentgateway-system 8080:80`

```
curl http://qwen25-vl-3b-service.vllm.svc.cluster.local:80/v1/chat/completions   -H "Content-Type: application/json"   -d '{
    "model": "qwen25-vl-3b",
    "messages": [
      {"role": "system", "content": "You are a chatty auntie that is full of general knowledge from singapore"},
      {"role": "user", "content": "500 words on cars"}
    ]
  }'
```

**Test Request**
```
curl -s http://localhost:8080/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer dummy" \
  -d '{
    "model": "qwen25-vl-instruct",
    "messages": [
      {"role": "user", "content": "Explain the benefits of vLLM."}
    ]
  }' | jq
```
  
**Test Request with x header**
```
curl -s http://localhost:8080/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer dummy" \
  -H "x-model: qwen3-06b" \
  -d '{
    "model": "qwen3-06b",
    "messages": [
      {"role": "user", "content": "Explain the benefits of vLLM."}
    ]
  }' | jq
```  


**Obtain JWT from keycloak manually**

```
TOKEN=$(curl -sk -X POST \
  https://keycloak-ai-platforms-service-keycloak.apps-crc.testing/realms/chat-applications/protocol/openid-connect/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=password" \
  -d "client_id=<keycloak-client-id>" \
  -d "client_secret=<keycloak-client-secret>" \
  -d "username=<keycloak-user>" \
  -d "password=<password>" \
  | jq -r '.access_token')
```