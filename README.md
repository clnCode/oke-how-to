# oke-howto
These are instructions on how to setup an Oracle Kubernetes Engine (OKE) cluster along with a Terraform module to automate part of that process.

## Prerequisites
First off you'll need to do some pre deploy setup.  That's all detailed [here](https://github.com/cloud-partners/oci-prerequisites).

## Clone the Module
Now, you'll want a local copy of this repo.  You can make that with the commands:

    git clone https://github.com/cloud-partners/oke-how-to.git
    cd oke-how-to/terraform
    ls

![](./images/01%20-%20git%20clone.png)

We now need to initialize the directory with the module in it.  This makes the module aware of the OCI provider.  You can do this by running:

    terraform init

This gives the following output:

![](./images/02%20-%20terraform%20init.png)

## Deploy
Now for the main attraction.  Let's make sure the plan looks good:

    terraform plan

That gives:

![](./images/03%20-%20terraform%20plan.png)

If that's good, we can go ahead and apply the deploy:

    terraform apply

You'll need to enter `yes` when prompted.  The apply should take about five minutes to run.  Once complete, you'll see something like this:

![](./images/04%20-%20terraform%20apply.png)

## Viewing the Cluster in the Console
We can check out our new cluster in the console by navigating [here](https://console.us-phoenix-1.oraclecloud.com/containers/clusters)

![](./images/05%20-%20console%20cluster.png)

Similarly, the IaaS machines running the cluster are viewable [here](https://console.us-phoenix-1.oraclecloud.com/a/compute/instances).

![](./images/06%20-%20console%20iaas.png)

## Setup the Terminal
To interact with our cluster, we need `kubectl` on our local machine.  Instructions for that are [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/).  I'm a big fan of easy and on a mac, so I just ran:

    brew install kubectl

That gave me this:

![](./images/07%20-%20brew%20install%20kubectl.png)

We're also probably going to want `helm`.  Once again, brew is our friend.  Those on other platforms could take a look [here](https://github.com/helm/helm).

    brew install kubernetes-helm

That gave me this:

![](./images/08%20-%20brew%20install%20helm.png)

The terraform apply dumped a kubernetes config file called config.  By default, `kubectl` expects the config file to be in `~/.kube/config`.  So, we can put it there by running:

    mkdir ~/.kube
    mv config ~/.kube

We can make sure this all worked by running this command to check out the nodes in our cluster:

    kubectl get nodes

That should give something like:

![](./images/09%20-%20get%20nodes.png)

## Destroy the Deployment
When you no longer need the OKE cluster, you can run this to delete the deployment:

    terraform destroy

You'll need to enter `yes` when prompted.  Once complete, you'll see something like this:

![](./images/10%20-%20terraform%20destroy.png)
