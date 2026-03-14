🔧 Challenges Faced & Solutions
<a name="worker-connection"></a>

1. Worker Node Couldn't Connect to Master
Problem: Worker kept showing "Failed to validate connection to cluster"

Solution: Fixed security group inbound rules — port 6443 source needed to be the security group itself, not a different group ID.

Commands used:

bash
# On master, verified k3s was running
sudo systemctl status k3s

# Checked if port 6443 was listening
sudo ss -tuln | grep 6443
<a name="invalid-token"></a>

2. Invalid Token Error
Problem: Worker failed with "invalid token CA hash length"

Solution: Generated fresh token from master using sudo cat /var/lib/rancher/k3s/server/node-token and used exact copy.

Commands used:

bash
# On master, get fresh token
sudo cat /var/lib/rancher/k3s/server/node-token

# On worker, rejoin with exact token
curl -sfL https://get.k3s.io | K3S_URL=https://172.31.24.109:6443 K3S_TOKEN=<fresh-token> sh -s - agent --node-name k3s-server2
<a name="dashboard-nodes"></a>

3. Dashboard Not Showing Nodes
Problem: Dashboard loaded but nodes section was empty

Solution: Created cluster role binding to give dashboard admin permissions.

Commands used:

bash
# Create cluster role binding
kubectl create clusterrolebinding admin-user-binding --clusterrole=cluster-admin --serviceaccount=kubernetes-dashboard:admin-user

# Generate fresh token for login
kubectl -n kubernetes-dashboard create token admin-user
<a name="app-access"></a>

4. App Not Accessible from Browser
Problem: nginx welcome page wouldn't load

Solution: Added inbound rule for NodePort range 30000-32767 to security group.

AWS Security Group rule added:

Type: Custom TCP

Protocol: TCP

Port range: 30000-32767

Source: 79.205.194.118/32 (my IP)

Description: NodePort services

Verification:

bash
# Check service port
kubectl get svc nginx
# Output: 80:31779/TCP

# Accessed in browser: http://worker-ip:31779
<a name="kubectl-hanging"></a>

5. kubectl Hanging
Problem: kubectl get nodes would hang indefinitely

Solution: Set KUBECONFIG environment variable correctly and fixed permissions on kubeconfig file.

Commands used:

bash
# Set KUBECONFIG environment variable
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml

# Fix file permissions
sudo chmod 644 /etc/rancher/k3s/k3s.yaml

# Make permanent
echo "export KUBECONFIG=/etc/rancher/k3s/k3s.yaml" >> ~/.bashrc
source ~/.bashrc

# Test it works
kubectl get nodes
<a name="sg-misconfiguration"></a>

6. Security Group Misconfiguration (Initial Setup)
Problem: Worker couldn't reach master because security groups had wrong source.

Solution: In AWS console, discovered inbound rules were pointing to a different security group ID. Fixed by ensuring all internal cluster rules had source set to the correct security group ID.

Before fix:

Port 6443 → source: sg-04710b2b2127ed95... (WRONG)

After fix:

Port 6443 → source: sg-081e3c89da29080b4 (CORRECT)

Port 10250 → source: sg-081e3c89da29080b4

Port 8472 (UDP) → source: sg-081e3c89da29080b4

<a name="elastic-ip"></a>

7. Elastic IP Association
Problem: After restarting EC2 instances, public IPs changed, breaking SSH access.

Solution: Allocated Elastic IPs to both master and worker nodes for permanent public IPs that survive reboots.

Steps taken:

Created two Elastic IPs in AWS console

Associated one with master node

Associated one with worker node

Updated SSH config with new permanent IPs

<a name="key-takeaways"></a>

🎯 Key Takeaways
Security groups are critical — always double-check sources

Tokens must be exact — no extra spaces or characters

Kubernetes permissions need explicit binding for dashboards

NodePort ranges must be opened in cloud firewalls

Elastic IPs save you from losing access after restarts

These challenges taught me more than any tutorial could. Every error was a learning opportunity! 💪

🔗 Quick Navigation
Worker Connection Issue

Invalid Token Error

Dashboard Nodes Not Showing

App Access Issue

kubectl Hanging

Security Group Misconfiguration

Elastic IP Issue

Key Takeaways

