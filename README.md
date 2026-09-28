<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Launch a Kubernetes Cluster

**Project Link:** [View Project](https://nextwork.ai/projects/aa8525df-82a0-5697-8721-a03b2db2c635)

**Author:** Abdul Hussein  
**Email:** abdulhussein@hotmail.se

---

![Image](https://nextwork.ai/authentic_blue_lucky_ferret/uploads/aa8525df-82a0-5697-8721-a03b2db2c635_e5f6g7h8)

## Introducing Today's Project!

In this project, I will launch a Kubernetes cluster using Amazon EKS, creating it with eksctl from an EC2 instance, because I want to learn the basics of Kubernetes and how it runs on AWS. Along the way I will set up the permissions the cluster needs, track its creation in CloudFormation, and give myself access to it in the EKS console.

## What is Kubernetes?

Kubernetes is a container orchestration platform, which is a fancy way to say that it coordinates containers so they're running smoothly across all your servers. It makes sure all your containers are running where they should, scales containers automatically to meet demand levels, and even restarts containers if something crashes.

Companies and developers use Kubernetes to run applications reliably at scale without managing each container by hand. It lets teams deploy new versions with no downtime, automatically scale up when traffic spikes and back down when it drops (saving costs), and recover on its own when a container or server fails.

I used eksctl to create an Amazon EKS cluster from a single command instead of setting up each piece of AWS infrastructure by hand. The create cluster command I ran defined the cluster name (nextwork-eks-cluster), a managed node group called nextwork-nodegroup, and the node type (t3.micro), a small, low-cost instance size suitable for a learning project. It also set the scaling settings, starting with 3 nodes and allowing the group to scale between a minimum of 1 and a maximum of 3, along with the Kubernetes version (1.33) and the region (eu-north-1) where everything was deployed.

I initially ran into two errors while using eksctl. The first happened because eksctl wasn't installed on my EC2 instance, so the terminal didn't recognize the command. I fixed it by downloading and installing eksctl, then moving it into my PATH so I could run it. The second error happened because my EC2 instance didn't have an IAM role giving it permission to create AWS resources, so eksctl couldn't build the cluster. I fixed it by creating an IAM role with the necessary permissions and attaching it to my instance. After that, the cluster creation command worked.

![Image](https://nextwork.ai/authentic_blue_lucky_ferret/uploads/aa8525df-82a0-5697-8721-a03b2db2c635_ff9bfc221)

## eksctl and CloudFormation

CloudFormation helped create my EKS cluster because eksctl doesn't build the resources directly. It generates CloudFormation templates and deploys them as stacks, so CloudFormation does the work of creating and connecting everything the cluster needs (the control plane, the IAM roles, and the node group) in the right order.

There was also a second CloudFormation stack for the node group (nextwork-nodegroup). eksctl splits the work into two stacks: the first creates the cluster and its networking, and the second creates the worker nodes that join the cluster once it exists. Keeping them separate means the node group can be updated, scaled, or deleted without touching the cluster itself.

The difference between a cluster and node group is that the cluster is the overall Kubernetes environment, including the control plane that AWS manages, which makes decisions like scheduling and keeping track of what should be running.

![Image](https://nextwork.ai/authentic_blue_lucky_ferret/uploads/aa8525df-82a0-5697-8721-a03b2db2c635_w3e4r5t6)

## The EKS console

I had to create an IAM access entry in order to view and interact with my cluster from the EKS console. An access entry is a link between an IAM identity and a cluster that defines who can access it and with which permissions. I set it up in the EKS console by opening the Access tab, creating an entry for my IAM user, and attaching the AmazonEKSClusterAdminPolicy.

It took about 15 to 20 minutes to create my cluster. Since I'll create this cluster again in the next project of this series, maybe this process could be sped up if I saved the settings in a YAML config file and reused it with eksctl create cluster -f, so I don't have to retype the long command.

![Image](https://nextwork.ai/authentic_blue_lucky_ferret/uploads/aa8525df-82a0-5697-8721-a03b2db2c635_e5f6g7h8)

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/aa8525df-82a0-5697-8721-a03b2db2c635)*
