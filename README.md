# capstone

## Infrastructure (First step in the pipeline)
The infrastructure/ folder contains a relativly simple EKS cloudformation deployment. The EKS is only deployed once and will be updated automatically in the pipeline if there are changes in the "infrastructure/cloudformation.yaml" file.

## Docker (Second step in the pipeline)
A docker-container based on nginx is build and linted. The index.html is replaced with my own index.html which also contains the ${CIRCLE_WORKFLOW_ID} so you can see that the deployment was successfull. The image is pushed to dockerhub. The tag contains the \$\{CIRCLE_WORKFLOW_ID\} so there is a new tag for each build.

## Deployment (Third step in the pipeline)
The "service.yaml" and "deployment.yaml" are deployed to kubernetes. The deployment.yaml contains a placeholder for the image-tag which is replaced by the \$\{CIRCLE_WORKFLOW_ID\}.

## Smoke-Test (Last step in pipeline)
After the deployment is applied to the cluster, the pipeline waits for the rollout to finish. This means kubernetes tries to replace old pod with new pods as soon as the new pods started successfully. If the new pods do not start within a timeout, the pipeline fails and the deployment is rolled back to be previously working version.
When the rollout was successfull the index.html is scraped with "curl" to check if it contains the \$\{CIRCLE_WORKFLOW_ID\} of the current pipeline. If this fails the deployment is also rolled back.