# Serverless Cloud Dictionary Application on AWS

---

## Overview

This project demonstrates the design and deployment of a **serverless cloud dictionary application** using AWS services.  
The application allows users to search for cloud-related terms and view their definitions through a web-based interface.

The architecture is built using a **React frontend** hosted on **AWS Amplify**, with a **serverless backend** powered by **AWS Lambda**, **API Gateway**, and **DynamoDB**.  
This design eliminates the need for traditional servers, ensuring scalability, low cost, and simplified maintenance.

### Application Features
- Users can **search for cloud terms** and instantly retrieve definitions.
- **Serverless backend** powered by AWS Lambda and API Gateway.
- **DynamoDB** serves as the fast and reliable NoSQL database.
- **AWS Amplify** hosts and serves the frontend React application.
- Designed for scalability and minimal operational overhead.

---

## Architecture Diagram

![Serverless Cloud Dictionary Application](https://github.com/user-attachments/assets/4d1d33a7-4450-4514-8ab4-0f517b534628)

*Figure 1: Serverless architecture integrating AWS Amplify, API Gateway, Lambda, and DynamoDB.*

### Workflow Description
1. The frontend React application is hosted on **AWS Amplify**, allowing users to search and view definitions through a web interface.  
2. The frontend sends **HTTP GET requests** to **API Gateway**, which exposes an endpoint (`/get-definition`) for backend communication.  
3. The API Gateway triggers the **Fetch Definition Lambda** function to process the request.  
4. **DynamoDB** serves as the database that stores all cloud terms and definitions.  
5. A **batch upload process** (via CLI or script) uses `batch-write-item` to insert multiple records into DynamoDB at once.

---

## AWS Services Used

### 1. AWS Amplify
- **Purpose:** Hosts the frontend React application.  
- **Functionality:** Provides a continuous deployment pipeline connected to a GitHub repository for automated updates.  
- **Role:** Acts as the web hosting and CI/CD layer for the application.

### 2. AWS Lambda
- **Purpose:** Serves as the backend compute layer for processing API requests.  
- **Functionality:**  
  - Handles requests to fetch cloud term definitions.  
  - Can be extended to handle new term submissions.  
- **Trigger:** Invoked by API Gateway upon receiving HTTP requests.  
- **Security:** Configured with IAM roles for read/write access to DynamoDB.

### 3. AWS API Gateway
- **Purpose:** Manages API endpoints and acts as the intermediary between frontend and backend.  
- **Functionality:**  
  - Routes incoming requests to the appropriate Lambda function.  
  - Provides RESTful API management with minimal configuration.  
- **Endpoint Example:**  
  `/get-definition`

### 4. AWS DynamoDB
- **Purpose:** Stores and retrieves cloud-related terms and their definitions.  
- **Functionality:**  
  - Acts as a NoSQL key-value store for quick data retrieval.  
  - Supports batch upload of terms through CLI scripts.  
- **Example Table Schema:**  
  - `Term` (Partition Key)   

### 5. AWS Identity and Access Management (IAM)
- **Purpose:** Manages permissions and access control.  
- **Functionality:**  
  - Defines roles and policies for Lambda, API Gateway, and DynamoDB.  
  - Ensures least-privilege access across all integrated services.

---

## Workflow

1. **Frontend Hosting:**  
   React frontend hosted on AWS Amplify connects with GitHub for automatic deployment.  

2. **API Invocation:**  
   When a user searches for a term, the frontend sends an API request via API Gateway.  

3. **Lambda Execution:**  
   API Gateway triggers the Fetch Definition Lambda function, which queries DynamoDB.  

4. **Data Retrieval:**  
   DynamoDB returns the matching term’s definition to the Lambda function.  

5. **Response Delivery:**  
   Lambda sends the result back to API Gateway, which returns the response to the frontend application.  

6. **Batch Upload:**  
   Admins or developers can upload bulk term definitions using a JSON dataset and the `batch-write-item` operation.

---

## Implementation Steps

### 1. Frontend Setup (AWS Amplify)
- Create a new **React application** or clone an existing one from GitHub.  
- Initialize AWS Amplify hosting via the Amplify Console.  
- Connect your GitHub repository for continuous integration and deployment.  
- Deploy the frontend app to the Amplify hosting environment.

### 2. Database Configuration (DynamoDB)
- Create a **DynamoDB table** with `Term` as the primary key.  
- Add a sample dataset manually or use a batch upload script:  
  ```bash
  aws dynamodb batch-write-item --request-items file://dataset.json
  ```

---

### 3. Lambda Function Creation
- Create a **Lambda function** (e.g., `fetchDefinitionLambda`) in the **AWS Lambda Console**.
- Choose **Python** or **Node.js** runtime for the function.
- Add logic to query **DynamoDB** based on user input (the searched term).
- Attach an **IAM Role** granting the following permissions to only read from the dynamodb database

---

### 4. API Gateway Configuration
- Create an **API Gateway REST API** to connect the frontend and backend.
- Add a new resource:  
```bash
/get-definition
```
- Create a **GET method** and integrate it with the `fetchDefinitionLambda` function.
- Deploy the API to a new or existing **stage**.
- Note the **API endpoint URL** for later use in the frontend application.

---

### 5. Frontend Integration
- In your **React application**, update the API endpoint URL to point to the deployed API Gateway endpoint.
- Implement frontend logic to:
- Capture the search term input from the user.
- Send the request to the `/get-definition` API.
- Display the returned definition from DynamoDB on the user interface.

---

### 6. Testing
- Deploy all AWS resources and verify end-to-end functionality.
- Steps to test:
1. Search for a term using the Amplify-hosted frontend application.
2. Confirm that the API Gateway triggers the Lambda function.
3. Verify the Lambda function queries DynamoDB successfully.
4. Ensure the definition is displayed correctly in the web UI.

---

## Conclusion
This project demonstrates a **fully serverless web application** leveraging AWS managed services for frontend hosting, backend logic, and data storage.  

By integrating **AWS Amplify**, **Lambda**, **API Gateway**, and **DynamoDB**, the architecture delivers:

- A scalable and cost-effective solution.  
- Real-time search functionality for cloud-related terms.  
- Seamless CI/CD integration through GitHub and Amplify.

![image](https://github.com/user-attachments/assets/b2f9dfea-b657-4a42-808a-b3461d525033)


