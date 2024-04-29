# Provisioning a Hosted Control Plane with Ansible Automation Platform
This repository details the steps to provision an HCP on your Openshift cluster using Ansible Automation Platform (AAP).


  
##Prerequisites:

1. An existing AAP instance
2. An OpenShift cluster with administrative access
3. A Git repository containing the hcp-install playbook

## Steps:

1. Create an AAP Project:

  - Log in to your AAP web interface.
  - Navigate to the Projects section.
  - Click Create Project.
  - Provide a descriptive name for your project (e.g., hcp-provisioning).
  - Click Create.

2. Create a Custom Credential Type: Create a custom credential type named kubeconfig to store your OpenShift cluster access information. This avoids directly exposing the access token in your playbooks.
  - Go to the Administration section within AAP.
  - Click on Credential Types.
  - Click Create Credential Type.
  - Provide a name (e.g., kubeconfig).
  - Paste the following in input configuration field:
      - fields:
        

      
