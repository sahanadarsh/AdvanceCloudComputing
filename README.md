# AdvanceCloudComputing
# Microservices to fetch weather info and send alerts to user
 - Developed Backend, Poller and Notifier microservices with separate RDS instance to store data
 - Backend manages users and publishes user configured watch & alert conditions using kafka to Poller. Poller fetches from weather API using zipcode and publishes weather and alert conditions to Notifier. Notifier sends alert to user based on the user’s alert condition.
 - Implemented continuous deployment pipeline for all the 3 web applications using Jenkins on EC2 instance
 - Implemented log forwarding and metrics for all 3 microservices and deployed on AWS, GKE and Azure
# Helm charts
 - Developed helm chart to deploy all the above 3 microservices on kubernetes cluster with replica count 3 using Rolling Update deployment strategy and with zero max unavailable value
 - Developed helm chart to implement HTTP based Readiness & Liveness probes for all the 3 web applications
 - Developed Helm chart to add roles, role bindings, service account resources to all 3 applications
# Infrastructure as Code with Ansible and Terraform
 - Automated setup and teardown networking components such as VPC, subnets, route table, internet gateway, security groups, EC2 instances, RDS instances, Kubernetes cluster on high availability zones and established VPC peering between RDS instances and Kubernetes cluster using Ansible playbooks and Terraform
 - Setup and teardown Jenkins on EC2 instance to implement CI using Ansible Playbooks and Terraform
