# 🔧 Challenges Faced & Solutions

## 1. Worker Node Couldn't Connect to Master
**Problem:** Worker kept showing "Failed to validate connection to cluster"
**Solution:** Fixed security group inbound rules — port 6443 source needed to be the security group itself, not a different group ID.

## 2. Invalid Token Error
**Problem:** Worker failed with "invalid token CA hash length"
**Solution:** Generated fresh token from master using `sudo cat /var/lib/rancher/k3s/server/node-token` and used exact copy.

## 3. Dashboard Not Showing Nodes
**Problem:** Dashboard loaded but nodes section was empty
**Solution:** Created cluster role binding: `kubectl create clusterrolebinding admin-user-binding --clusterrole=cluster-admin --serviceaccount=kubernetes-dashboard:admin-user`

## 4. App Not Accessible from Browser
**Problem:** nginx welcome page wouldn't load
**Solution:** Added inbound rule for NodePort range 30000-32767 to security group.

## 5. kubectl Hanging
**Problem:** `kubectl get nodes` would hang indefinitely
**Solution:** Set KUBECONFIG environment variable correctly and fixed permissions on kubeconfig file.