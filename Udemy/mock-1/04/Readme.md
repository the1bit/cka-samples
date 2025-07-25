# Mock Exam 1 - 04

## Task: Expose application

Create a service messaging-service to expose the messaging application withun the cluster on port 6379.

Use imerative commands to create.

Requirements:
- Service: messaging-service
- Port: 6379
- Service type: ClusterIP
- Use the right labels


## Solution Steps

1. **Create the service using kubectl:**

   ```sh
   kubectl expose pod messaging --name=messaging-service --port=6379 --type=ClusterIP
   ```