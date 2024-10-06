# HHA504_assignment_functions

# Serverless Functions Assignment: Azure & GCP

## Overview
This project involves deploying serverless functions on both **Azure** and **Google Cloud Platform (GCP)**, then automating the invocation of these functions using **GitHub Actions**. The goal is to create HTTP-triggered functions, automate their execution with cron jobs, and document the process, issues encountered, and solutions.

## Requirements

- **Create and deploy a serverless function** on both **Azure** and **GCP**.
- **Automate the function execution** via a **GitHub Actions** cron job.
- Document the steps taken, including any issues encountered and solutions.
- Include screenshots of key steps.

## Azure Function Setup

### 1. **Creating the Function App on Azure**
- Open the Azure portal, and create a new Function App using the **Consumption Plan**.
- The function app was set up with the following configurations:
  - **Runtime stack**: Node.js
  - **Region**: East US
  - **OS**: Linux

  ![Azure Function App Creation](Functions/Azure%20Function%20App%20Creation.png)

### 2. **Creating the Azure Function**
- I created an HTTP-triggered function named `HelloWorldFunction`.
- The function code was written in **JavaScript**:
  ```javascript
  module.exports = async function (context, req) {
      context.log('JavaScript HTTP trigger function processed a request.');
      const name = (req.query.name || (req.body && req.body.name));
      const responseMessage = name
          ? "Hello, " + name + ". This HTTP triggered function executed successfully."
          : "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.";
      context.res = {
          status: 200,
          body: responseMessage
      };
  };
![Azure Code + Test Screenshot](Functions/Azure%20Code%20+%20Test%20Screenshot.png)

![Azure Test:Run Function](Functions/Azure%20Test%3ARun%20Function.png)

### 3. **Automating the Function with GitHub Actions**
- I created a **GitHub Actions** workflow to run the Azure function automatically using a cron job.
- The workflow file is stored in `.github/workflows/trigger.yml`:
  ```yaml
  name: Scheduled Azure Function Test
  on:
    schedule:
      - cron: "0 0 * * *" # This runs the job daily at midnight (UTC)
  jobs:
    trigger-azure-function:
      runs-on: ubuntu-latest
      steps:
        - name: Trigger Azure Function
          run: curl -X GET "https://nomnomcookie27.azurewebsites.net/api/HelloWorldFunction?code=gqlIao3UvbItxnao_C_-iq8rA9qEcLFBpGmuHTnV3tAPAzFuwUajEg%3D%3D"
### Azure Issues Encountered and Solutions

- **Deployment Failures**: 
  - Issue: The deployment failed due to storage account misconfigurations.
  - Solution: This was resolved by ensuring the correct configurations for the storage account during function setup.

- **Authentication Settings**: 
  - Issue: Errors related to incorrect authentication settings were encountered.
  - Solution: Adjusted the permissions to allow HTTP requests and ensured the proper API and role-based permissions were set for the function.

## GCP Function Setup

## GCP Function Setup

### 1. **Creating the Function App on GCP**

- Open the Google Cloud Platform (GCP) console, and create a new Cloud Function App.
- The function app was set up with the following configurations:
  - **Runtime stack**: Node.js
  - **Region**: US Central (Iowa)
  - **Trigger type**: HTTP
![GCP Serverless Dashboard](Functions/GCP%20Serverless%20Dashboard.png)

### 2. **Creating the GCP Function**

- I created an HTTP-triggered function named `helloWorld`.
- The function code was written in **JavaScript**:

```javascript
exports.helloWorld = (req, res) => {
  const name = req.query.name || req.body.name || 'World';
  res.status(200).send(`Hello, ${name}! This HTTP triggered function executed successfully.`);
};
```
![GCP Function Trigger](Functions/GCP%20Function%20Trigger.png)

### 3. Automating the GCP Function with GitHub Actions (Optional)

- You can automate the GCP function deployment with GitHub Actions or manually test it through the GCP console.

## GCP Issues Encountered and Solutions
![404 URL Error](Functions/404%20Url%20Error.png)
- **404 Not Found Errors**:
  - Issue: After deployment, the function URL returned a 404 error.
  - Solution: This was resolved by enabling the necessary APIs for the project, such as Cloud Functions, Cloud Build, and Cloud Logging.

- **Deployment Failures**:
  - Issue: The function failed to deploy due to an incorrect entry point.
  - Solution: Ensured that the entry point in the GCP console matched the function name in the code (`helloWorld`).

- **Permission Errors**:
  - Issue: The function could not be accessed due to permission issues.
  - Solution: Allowed unauthenticated invocations for the function in the GCP console.
  
## Conclusion

In this assignment, I successfully deployed serverless functions on both Azure and GCP using HTTP triggers, automated them using GitHub Actions, and resolved several issues during the setup and deployment processes. The documentation and the screenshots provided capture all the major steps and solutions encountered during the assignment.








