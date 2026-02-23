
# Action Trigger Repository

This repository acts as the **source** of Git events (Pushes, Pull Requests, and Merges) for the Webhook Receiver Assessment.

**Note:** This repository contains dummy code/files intended solely to trigger GitHub Actions/Webhooks.

## 🔗 How to Connect

To test the Webhook Receiver application with this repository, you must configure a webhook.

**⚠️ Important:** You generally cannot add webhooks to a repository you do not own. **Please Fork this repository** to your own account to configure the settings.

### Configuration Steps

1.  **Fork** this repository.
2.  Navigate to **Settings** > **Webhooks** > **Add webhook**.
3.  **Payload URL:** Paste the Ngrok URL from your running Webhook Receiver app and append `/webhook/receiver`.
    *   *Format:* `https://<your-ngrok-id>.ngrok-free.app/webhook/receiver`
4. **Also make sure you have authenticated your ngrok before next steps**
5.  **Content type:** Select `application/json` (Crucial!).
6.  **Secret:** Leave blank.
7.  **Which events would you like to trigger this webhook?**
    *   Select **"Let me select individual events"**.
    *   Check **Pushes**.
    *   Check **Pull requests**.
8.  Click **Add webhook**.

## ⚡ How to Trigger Events

Once the webhook is added and the green checkmark appears (indicating a successful ping), you can generate events:

### 1. Trigger a PUSH
Edit any file (e.g., `README.md`), commit, and push to the branch.
```bash
git add .
git commit -m "Testing Push Event"
git push origin main
```

### 2. Trigger a PULL REQUEST
Create a new branch, push changes, and open a PR via the GitHub UI.
```bash
git checkout -b new-feature
# make changes
git commit -m "New feature"
git push origin new-feature
```
*Then go to GitHub and click "Compare & pull request".*

### 3. Trigger a MERGE (Brownie Points)
1.  Go to the Pull Request you just created on GitHub.
2.  Click the **"Merge pull request"** button.
3.  Confirm the merge.

## 🔍 Verification
Check the Dashboard UI running at `http://localhost:5000`. The events should appear within 15 seconds of the action. 