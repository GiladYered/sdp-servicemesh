# LAB 01: Installing Linkerd

## Description

In this lab we will install Linkerd 2.x into a GKE cluster

## Instructions

1. Access to your workstation (the instructor will provide you with the connection details)

```
ssh sela@<your-ip>
```

---

2. Let's install kubectl in the workstation server

```
sudo snap install kubectl --classic
```

---

3. Let's use gcloud CLI to configure kubectl and get access to the cluster

```
gcloud container clusters get-credentials $(hostname) --zone $(gcloud compute instances list $(hostname) --format "value(zone)") --project devops-course-architecture
```

---

4. Test the kubectl configuration by running the following command

```
kubectl get nodes
```

---

5. Install the linkerd CLI

```
curl -sL https://run.linkerd.io/install | sh
```

---

6. Add linkerd to your path with

```
echo 'export PATH=$PATH:$HOME/.linkerd2/bin' >> ${HOME}/.bash_profile
source ${HOME}/.bash_profile
```

---

7. Verify the CLI is running correctly with the command below (you should see the CLI version, and also Server version: unavailable. This is because you haven't installed the control plane on your cluster. Don't worry, you'll be installing the control plane soon)

```
linkerd version
```

---

8. Validate your kubernetes clusters

```
linkerd check --pre
```

---

9. Install Linkerd onto the cluster

```
linkerd install | kubectl apply -f -
```

---

10. Verify the installation

```
linkerd check
```

---

11. Grant permissions for the linkerd tap role

```
kubectl create clusterrolebinding \
  $(whoami)-tap-admin \
  --clusterrole=linkerd-linkerd-tap-admin \
  --user=$(gcloud config get-value account)
```

---

12. Finally, let's see what has been installed

```
kubectl -n linkerd get all
```
