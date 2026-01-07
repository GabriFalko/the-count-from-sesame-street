# ArangoDB FIPS Deployment Guide

This guide provides step-by-step instructions for deploying ArangoDB with FIPS (Federal Information Processing Standards) compliance in a Kubernetes environment.

## Prerequisites

- Kubernetes cluster configured and accessible
- `kubectl` command-line tool installed and configured
- FIPS-compliant ArangoDB image available
- Appropriate permissions to deploy resources in your Kubernetes cluster

## Deployment Steps

### Step 1: Prepare the FIPS-enabled ArangoDB Configuration

Create the necessary Kubernetes manifests with FIPS-compliant settings. Ensure your ArangoDB image is FIPS-certified and properly configured.

### Step 2: Deploy ArangoDB to Kubernetes

Apply your Kubernetes manifests to deploy the ArangoDB instance:

```bash
kubectl apply -f arangodb-deployment.yaml
```

### Step 3: Verify Deployment Status

Check that the ArangoDB pods are running successfully:

```bash
kubectl get deployments
kubectl get pods
```

### Step 4: Validate FIPS Configuration

Verify that FIPS mode is enabled in your ArangoDB instance by checking the configuration and logs.

### Step 5: Access the ArangoDB Instance

Before you can connect to the ArangoDB instance using curl or other tools, you need to set up port forwarding from your local machine to the pod running in Kubernetes.

#### 5.1: Find the Running Server Pod

First, identify the name of the running ArangoDB server pod:

```bash
kubectl get pods -l app=arangodb
```

This command will list all pods with the label `app=arangodb`. Note the exact pod name from the output (e.g., `arangodb-simple-single-67f8b9c5d4-xk2lm`).

#### 5.2: Set Up Port Forwarding

Open a new terminal window or tab in the same workspace. Then, set up port forwarding to the ArangoDB pod:

```bash
kubectl port-forward pod/arangodb-simple-single-67f8b9c5d4-xk2lm 8529:8529
```

Replace `arangodb-simple-single-67f8b9c5d4-xk2lm` with the actual pod name you found in step 5.1.

**Important:** Keep this terminal window open and running. The port-forward connection must remain active for the following steps to work.

#### 5.3: Test the Connection

Now you can access the ArangoDB instance from your local machine. In your original terminal (not the one running port-forward), test the connection:

```bash
curl http://localhost:8529/_api/version
```

This should return the ArangoDB version information, confirming that the connection is working properly.

### Step 6: Configure and Use ArangoDB

With the port forwarding in place, you can now:

- Access the ArangoDB web interface at `http://localhost:8529`
- Connect using the ArangoDB client tools
- Run administrative commands and queries

### Step 7: Security Best Practices

- Ensure all connections use proper authentication
- Verify FIPS mode is active by checking `/_admin/server/availability`
- Regularly update to the latest FIPS-compliant ArangoDB versions
- Monitor logs for any FIPS-related warnings or errors

## Troubleshooting

### Port Forward Connection Issues

If you experience connection issues:
- Verify the pod is running: `kubectl get pods`
- Check pod logs: `kubectl logs <pod-name>`
- Ensure no other process is using port 8529 locally

### FIPS Validation Issues

If FIPS mode is not active:
- Verify the ArangoDB image is FIPS-certified
- Check the deployment configuration for FIPS settings
- Review pod logs for FIPS-related errors

## Additional Resources

- [ArangoDB Documentation](https://www.arangodb.com/docs/)
- [Kubernetes Port Forwarding Guide](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/)
- [FIPS 140-2 Compliance Documentation](https://www.arangodb.com/security/)
