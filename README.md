# HelmDeployment
_This component was created as an output of the INCISIVE European project software, forming part of the final platform_

### Introduction
This repository contains the helm charts to deploy the BSC components in the INCISIVE infrastructure.

Concretely, there are three different helm charts:
- Orchestrator: It is the one in charge of managing the placement and behaviour of all AI Engines' executions.
- MaaS: It is the one in charge of providing the backend to the distinct organizations for storing and retrieving their AI Engines and models along their metadata.
- Kafka: It is the one in charge of providing the platform to the AI Engines to communicate during the parallel training.

### Components

#### Orchestrator
The Orchestrator helm chart deploys the following two modules:
- Service: It is the module that serves the API and performs all the logic of the Orchestrator component. It comes with the next Kubernetes objects:
  - Role, RoleBinding, ServiceAccount -> give the required rights for working with the Kubernetes API
  - Service -> expose the Orchestrator API to other INCISIVE components
  - ConfigMap, Secret -> provide the required configuration parameters
  - Deployment -> deploy the needed containers. Specifically it deploys three, one init container for initializing the database and two normal containers, one for requesting the executions status to the Kubernetes API and the other one for doing all the rest of the component logic
- Database: It is the module that manages the Orchestrator relational database. It comes with the next Kubernetes objects:
  - Service -> expose the database to the Service module of the Orchestrator
  - ConfigMap, Secret -> provide the required configuration parameters
  - Storage -> configure the storage for the database
  - Deployment -> deploy the needed containers
  
Also, the helm chart deploys two more ConfigMaps for the fl-client and fl-manager auxiliary modules that are used in the case of paralell training

#### MaaS
The MaaS helm chart deploys the following two modules:
- Service: It is the module that serves the API and performs all the logic of the MaaS component. It comes with the next Kubernetes objects:
  - Service -> expose the MaaS API to other INCISIVE components
  - ConfigMap, Secret -> provide the required configuration parameters
  - Deployment -> deploy the needed containers. Specifically it deploys two, one init container for initializing the database and one normal container to perform all the component logic
- Database: It is the module that manages the MaaS relational database. It comes with the next Kubernetes objects:
  - Service -> expose the database to the Service module of the MaaS
  - ConfigMap, Secret -> provide the required configuration parameters
  - Storage -> configure the storage for the database
  - Deployment -> deploy the needed containers
  
#### Kafka
The Kafka helm chart deploys the required Kubernetes' objects to create a Kafka cluster. For that purpose, we install the Strimzi helm chart as dependency and create an object of kind Kafka with the desired configuration parameters.
