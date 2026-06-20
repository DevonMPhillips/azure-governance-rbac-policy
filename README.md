# azure-governance-rbac-policy
Azure governance lab demonstrating RBAC, Azure Policy, resource tagging, cost management, and monitoring using Azure CLI and Microsoft Entra ID.

---
Project 3: Azure Governance, Policy, RBAC, and Cost Management

You are the Azure Administrator for DMP Consulting.

The company is growing and wants centralized governance across all Azure resources.

You need to:

1. Create a Management Group hierarchy
2. Apply policies at different scopes
3. Assign RBAC roles at different scopes
3. Demonstrate inheritance
4. Validate effective permissions

---
Lab Architecture

<img width="581" height="241" alt="project 3 Diagram drawio" src="https://github.com/user-attachments/assets/fc685eb2-6353-43df-a9fc-b5eb7f6eb61c" />

---
Create a Management Group

<img width="1012" height="578" alt="image" src="https://github.com/user-attachments/assets/88268864-bc1b-41a0-a7ec-e8ad8a3a2343" />

az account management-group create --name DMPConsulting-Management
<img width="867" height="397" alt="image" src="https://github.com/user-attachments/assets/f5438d7a-494d-4d58-8a17-ff99b61bfc03" />

<img width="1252" height="672" alt="image" src="https://github.com/user-attachments/assets/2a0c0d19-efe4-49e9-9bc3-b4d0be9f2be4" />

---
Move Subscription
Move DMPConsulting subscription to newly create management group
az account management-group subscription add --name "DMPConsulting-Management" --subscription "f92d2dac-adc4-4d2d-96c0-b0fa0c6bbb25"

<img width="1245" height="219" alt="image" src="https://github.com/user-attachments/assets/a21987f3-f623-4788-9d62-076145a3b67c" />

<img width="1257" height="576" alt="image" src="https://github.com/user-attachments/assets/0b5736fb-e2e3-481b-bf3d-7b2356ed7f73" />

---
Create a Subscription-Level Policy
Allowed Locations policy: east us, west us, central us

az policy assignment create --name "allowed-locations-assignment" --display-name "Allowed Locations" --policy "e56962a6-4747-49cd-b67b-bf8b01975c4c" --scope "/subscriptions/f92d2dac-adc4-4d2d-96c0-b0fa0c6bbb25" --params '{"listOfAllowedLocations": {"value": ["eastus", "westus", "centralus"]}}'

<img width="1257" height="637" alt="image" src="https://github.com/user-attachments/assets/29081a53-006e-4016-b219-285449878e95" />

Resource groups inhertis policies

<img width="1257" height="762" alt="image" src="https://github.com/user-attachments/assets/2b0d33d3-b283-43f4-9c70-ae8f6f092cf3" />

---
Create RBAC Assignments at Different Scopes
Create three test users.

Subscription Reader:
Can view all resources and resource groups, but cannot create, modify, or delete

az ad user create --display-name "Subscription Reader" --password "[Super Secret Password :P}" --user-principal-name "SubscriptionReader@devonphillips211gmail.onmicrosoft.com" && az role assignment create --assignee "SubscriptionReader@devonphillips211gmail.onmicrosoft.com" --role "Reader" --scope "/subscriptions/f92d2dac-adc4-4d2d-96c0-b0fa0c6bbb25"

<img width="1241" height="631" alt="image" src="https://github.com/user-attachments/assets/84d06397-cfcf-4312-b5d2-b0d19def68f9" />

<img width="1257" height="701" alt="image" src="https://github.com/user-attachments/assets/38b72b04-f6d4-4d03-be11-7e4537c794e5" />

DevContributor:
Can create resources, modify resources, and delete resources only inside Corporate-Dev-RG

az ad user create --display-name "DevContributor" --password "[Super Secret Password :P}" --user-principal-name "DevContributor@devonphillips211gmail.onmicrosoft.com" && az role assignment create --assignee "DevContributor@devonphillips211gmail.onmicrosoft.com" --role "Contributor" --scope "/subscriptions/f92d2dac-adc4-4d2d-96c0-b0fa0c6bbb25/resourceGroups/DMPConsulting-dev-RG"

<img width="1256" height="671" alt="image" src="https://github.com/user-attachments/assets/0b96a6aa-307d-4fb3-8753-a37c0f814fb4" />

<img width="1255" height="631" alt="image" src="https://github.com/user-attachments/assets/09279bae-2719-40b1-88ce-e77f376a82b1" />

StorageReader:

az ad user create --display-name "Storage Reader" --password "[Super Secret Password :P}" --user-principal-name "Storagereader@devonphillips211gmail.onmicrosoft.com" && az role assignment create --assignee "Storagereader@devonphillips211gmail.onmicrosoft.com" --role "Reader" --scope "/subscriptions/f92d2dac-adc4-4d2d-96c0-b0fa0c6bbb25/resourceGroups/DMPConsulting-Prod-RG/providers/Microsoft.Storage/storageAccounts/dmpstorage001"

<img width="1262" height="714" alt="image" src="https://github.com/user-attachments/assets/09392f0c-9809-45c0-aaf8-f11e635c5fd0" />

<img width="1252" height="802" alt="image" src="https://github.com/user-attachments/assets/67943981-6480-49e8-8243-a9e40554d36f" />

---

Ive succesfully mplemented Azure governance hierarchy using Management Groups, Subscription-level Policies, and RBAC assignments at multiple scopes to demonstrate access inheritance and least-privilege administration.

---
Lessons Learned

Management Groups sit above subscriptions.
Azure Policy inherits downward.
RBAC inherits downward.
Scope determines the reach of permissions.
Least privilege reduces risk.
Effective permissions must account for inheritance.
