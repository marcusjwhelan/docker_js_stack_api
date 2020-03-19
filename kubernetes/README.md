# Kubernetes Deployment

## Test kubernetes deployment

```bash
kubectl apply --validate=true --dry-run=true -f deployment.yml
kubeclt apply --validate=true -f deployment.yml
kubectl get services
```

Should get localhost for external IP and be able to go to localhost and get welcome.

Shut down test
```bash
kubectl delete deploy/api svc/api
```
