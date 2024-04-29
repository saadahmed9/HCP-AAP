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

          fields
             - id: kube_config
               type: string
               label: kubeconfig
               secret: true
               multiline: true
          required:
             - kube_config

  - Paste the following in Injector configuration field:
    
          env:
            KUBECONFIG: '{{ tower.filename.kubeconfig }}'
            K8S_AUTH_KUBECONFIG: '{{ tower.filename.kubeconfig }}'
          file:
            template.kubeconfig: '{{ kube_config }}'

    - Click Save.

3. Create an OpenShift Cluster Credential:
    - Go to the Credentials section within your newly created project.
    - Click Create Credential.
    - Provide a name
    - Choose kubeconfig as the credential type.
    - Paste the kubeconfig of the Hub Openshift cluster on which the HCP will be provisioned.

4. Create a Pull Secret Credential Type and a pull-secret credential.
    - Go to the Administration section within AAP.
    - Click on Credential Types.
    - Click Create Credential Type.
    - Provide a name (e.g., pull-secret).
    - Paste the following in input configuration field:

           fields:
              - id: pull_secret
                type: string
                label: pull_secret
                secret: true
                multiline: true
           required:
              - pull_secret

    - Paste the following in Injector configuration field:

          env:
            PULL_SECRET: '{{ tower.filename.sec }}'
          file:
            template.sec: '{{ pull_secret }}'


    - Go to the Credentials section within your newly created project.
    - Click Create Credential.
    - Provide a name
    - Choose pull-secret as the credential type.
    - Paste your pull-secret configuration.

5. Create a Job Template:
    - Go to the Templates section within your project.
    - Click Create Template.
    - Provide a name for your job template (e.g., hcp-deployment).
    - Under Inventory choose Demo Inventory which contains the localhost as Host.
    - Select the Project you created earlier, and choose the hcp-provisioning playbook.
    - Under Credentials, select the kubeconfig credential and the pull-secret credential you created earlier.

6. Launch the Job:
    - Once your job template is configured, click Launch.
    - Review the confirmation details and click Launch again to initiate the HCP deployment process on your OpenShift cluster using the hcp-install playbook.

7. Monitor Job Progress:
    - Navigate to the Jobs section within your AAP project.
    - You'll see the launched job with its status (running, successful, failed, etc.).

        

      
