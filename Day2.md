## Creating Pods

```bash
kubectl run nginx --image=nginx                
```

Pod with YAML

```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx.yaml              
```


