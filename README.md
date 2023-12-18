# Ingesting [Jira Software (Server)](https://www.atlassian.com/software/jira/download-journey) Resources

## Overview

In this example, you'll set up blueprints for managing projects and issues in your Jira Software (Server) account. Using a Python script, you'll make API calls to JIRA REST API to retrieve information about your projects and issues. Additionally, you'll configure webhooks to keep your Port entities updated automatically whenever there's an event in your Jira Software (Server) account. For this demo, we'll focus on subscribing to updates related to projects and issues.

> **_NOTE:_**  This script is designed to operate with Jira Software Server, the self-hosted version. If you are utilizing Jira Cloud, please refer to the [Jira Ocean Integration](https://docs.getport.io/build-your-software-catalog/sync-data-to-catalog/project-management/jira) for appropriate instructions.

## Getting started

Log in to your Port account and create the following blueprints:

### Project blueprint
Create the project blueprint in Port [using this json file](./resources/project.json)

### Issue blueprint
Create the repository blueprint in Port [using this json file](./resources/issue.json)


### Running the python script

To ingest data from your Jira Software (Server) account to Port, run the following commands: 

```
export PORT_CLIENT_ID=<ENTER CLIENT ID>
export PORT_CLIENT_SECRET=<ENTER CLIENT SECRET>
export JIRA_USERNAME=<ENTER JIRA USERNAME>
export JIRA_PASSWORD=<ENTER JIRA PASSWORD>
export JIRA_API_URL=http://localhost:8080

git clone https://github.com/port-labs/jira-server-data.git

cd jira-server-data

pip install -r ./requirements.txt

python app.py
```

The list of variables required to run this script are:
- `PORT_CLIENT_ID`
- `PORT_CLIENT_SECRET`
- `JIRA_API_URL` - Jira server host such as `http://localhost:8080`
- `JIRA_USERNAME` - Jira username to use when accessing the Jira Software (Server) resources
- `JIRA_PASSWORD` - Jira account password or token to use when accessing the Jira resources

## Port Webhook Configuration

Webhooks are a great way to receive updates from third party platforms, and in this case, Jira Software (Server). To [create a jira webhook](https://developer.atlassian.com/server/jira/platform/webhooks/), you will first need to generate a webhook URL from Port.

Follow the following steps to create a webhook:
1. Navigate to the **Builder** section in Port and click **Data source**;
2. Under **Webhook** tab, click **Custom integration**;
3. In the **basic details** tab, you will be asked to provide information about your webhook such as the `title`, `identifier` `description`, and `icon`;
4. In the **integration configuration** tab, copy and paste the [webhook configuration file](./resources/webhook.json) into the **Map the data from the external system into Port** form;
5. Take note of the webhook `URL` provided by Port on this page. You will need this `URL` when subscribing to events in Jira;
6. Test the webhook configuration mapping and click on **Save**;



## Subscribing to Jira webhook events
1. From your Jira Software (Server) account, open settings and then click on the **System** link;
2. Look for the **Webhook** section in the sidebar menu. It is closer to the bottom;
3. Click the **Create a webhook** button to create a webhook for the server; 
5. Input the following details:
    1. `Name` - use a meaningful name such as Port Webhook;
    2. `URL` - enter the value of the webhook `URL` you received after creating the webhook configuration in Port;
    3. `Description` - A useful description;
    4.  `Events` 
        - Under **Project** select `created`, `updated`
        - Under **Issue** select `created`, `updated`
        - Select any other event based on your case, and make the relevant changes in the code to trap them.
6. Click **Save** to save the webhook;

For a visual walkthrough of the steps, follow [this documentation](https://developer.atlassian.com/server/jira/platform/webhooks/)

All set! When any changes occur in your project or issues in Jira, a webhook event will be triggered to the URL provided by Port. Port will then parse the events based on the mapping and subsequently update the catalog entities.
