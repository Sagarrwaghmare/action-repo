
---

# GitHub Action Repository

This repository is used as the source of Git events (pushes, pull requests, etc.) for the **Git Event Webhook Dashboard** project.

The purpose of this repository is to be configured with a webhook that sends notifications about its activity to a separate listener application.

## How It Works

The data flow is simple:

1.  You perform a Git action in this repository (e.g., `git push`).
2.  GitHub triggers a webhook event.
3.  The event is sent over the internet to a public URL provided by **Ngrok**.
4.  Ngrok tunnels this data securely to the listener application running on a local machine.
5.  The event appears on the dashboard.

## Setup Instructions

To connect this repository to your local dashboard application, you must configure a webhook.

### Step 1: Get the Webhook URL from Ngrok

Before configuring this repository, the main **Webhook Dashboard** application must be running, and you must have an active Ngrok tunnel pointing to it.

1.  On the machine running the dashboard application, start Ngrok to expose port 5000:
    ```bash
    ngrok http 5000
    ```

2.  Ngrok will provide a public HTTPS URL. **Copy this URL.** It will look something like this:
    ```
    https://a1b2-c3d4-e5f6.ngrok-free.app
    ```

### Step 2: Configure the GitHub Webhook

Now, you will tell this repository where to send its events.

1.  In this GitHub repository, navigate to **Settings** > **Webhooks**.

2.  Click the **Add webhook** button.

3.  Fill out the form with the following details:

    *   **Payload URL**: Paste your HTTPS Ngrok URL from Step 1 and append the endpoint path `/webhook/receiver`.
        > **Example**: `https://a1b2-c3d4-e5f6.ngrok-free.app/webhook/receiver`

    *   **Content type**: This is critical. Change it to **`application/json`**.

    *   **Which events would you like to trigger this webhook?**:
        *   Select **"Let me select individual events"**.
        *   Check the boxes for **Pushes** and **Pull requests**.

4.  Leave everything else as default and click **Add webhook**.

### Step 3: Verify the Connection

After adding the webhook, GitHub will send a "ping" event. You can verify the connection in three places:

*   **On GitHub**: In the Webhooks settings, you should see your new webhook with a green checkmark, indicating the last delivery was successful.
*   **In the Ngrok Terminal**: You will see a log line showing a `POST /webhook/receiver` with a `200 OK` status.
*   **In the Dashboard App Terminal**: You will see the "EVENT RECEIVED" log message from the Flask application.

## Usage

Once set up, simply use Git as you normally would with this repository.

*   **To test a PUSH event**:
    ```bash
    git commit -m "Test push event" --allow-empty
    git push origin main
    ```

*   **To test a PULL REQUEST event**:
    1.  Create and push a new branch.
    2.  Go to the repository on GitHub and open a new Pull Request.

*   **To test a MERGE event**:
    1.  Merge the Pull Request you created on the GitHub website.

Each of these actions will now be sent through the webhook and should appear on the dashboard within seconds.

---
*If the Ngrok tunnel is restarted, it will generate a new public URL. You must update the **Payload URL** in the webhook settings with the new address.*