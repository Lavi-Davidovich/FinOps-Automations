# GCP-ExistingVM-Tagging-Script
**1. Create a Service Account with permission**

_Service account details:_

- Cloud Run Invoker

- create-labels-for-instance

- Logs Writer

- Monitoring Metric Writer

- Storage Object Viewer



_Service account Trigger:_

- Cloud Run Invoker

- create-labels-for-instance

- Logs Viewer

- Storage Object Viewer for the bucket that contain the file labels


---------------------------------------

**2. Create a Pub/Sub Topic**
Before creating a log sink, you need a Pub/Sub topic to which logs will be published.
1.	Go to Google Cloud Console.
2.	Navigate to Pub/Sub:
- From the sidebar, select Pub/Sub.
- Click Create Topic.
3.	Create a Topic:
- Enter a name for the topic (e.g., vm-create-logs).
- Click Create.


---------------------------------------


**3. Create a Cloud Function**
Next, you'll need to create a Cloud Function that will be triggered by the Pub/Sub message.


Create a Function:
- Click Create Function.
- Give it a name (e.g., vmCreationFunction).
- Select Pub/Sub as the trigger type.
- Choose the topic you created earlier (vm-create-logs).
- Runtime : Python 3.12
- Entry point : apply_labels
- google-cloud-compute, google-cloud-storage, functions-framework==3.*


---------------------------------------

**4.	Create a Log Sink**
- Click Create Sink.
- Enter a name for your sink (e.g., vm-creation-sink).
- Under Sink Destination, choose Cloud Pub/Sub.
- Select the Pub/Sub topic you created earlier (vm-create-logs).
- Create the Filter for VM Creation Logs:
- In the Create Filter section, use the following filter to capture VM creation logs:
"compute.instances.insert" AND operation.last="true"
