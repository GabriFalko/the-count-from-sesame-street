# The Count (from Sesame Street): Slack Notification Bot
Welcome to **The Count** project! This repository contains a GitHub Actions-powered bot that helps your team stay updated by sending Slack messages whenever a new image is completed in another project.

---

## What does this bot do?

- **Listens for updates:** When an issue labeled "new image" is closed and completed in another repository, this bot is notified automatically.
- **Sends Slack messages:** The bot posts a friendly message to your chosen Slack channel with details about the new image, including its name, number, and completion time.

---

## How does it work?

1. **Automatic Triggers:**  
   Another repository in your organization (for example, a project where images are tracked) has an automated workflow. When someone closes an issue labeled "new image," it notifies this bot.

2. **Message Formatting:**  
   The bot receives all the information it needs (image name, number, and time) from the other repository. It then creates a clear and simple message.

3. **Slack Notification:**  
   The bot uses Slack's "Incoming Webhook" feature to send the message to your team's Slack channel. No manual steps are needed.

<p align="center">
  <img src="/assets/the-count-diagram.drawio.svg" default="the-count-diagram.drawio">
</p>

Model of **Prototype** which may include unfinished features

---

## Structure of This Repository

- `.github/workflows/post-to-slack.yml`  
  This file contains the GitHub Actions workflow. It listens for events from the other repository and sends messages to Slack.

- `README.md`  
  (This file) Explains what the bot does and how it works.

- **No code or server needed:**  
  Everything is handled by GitHub Actions (automated scripts), so there’s no need to run or maintain a separate application.

---

## How does the notification process work?

1. **In the image-tracking repository:**
   - When a new image is finished (issue closed with the "new image" label), a workflow sends the details to this bot.

2. **In this repository:**
   - The workflow receives the information and posts it to Slack using the webhook.

3. **On Slack:**
   - Your team sees a message like:
     ```
     Greeting Message
     Number: [Image Number]
     Name: [Image Name]
     Issue: [Link to related Issue]
     Completed at: [Date & Time]
     ```

---

## What do I need to set up?

- **Slack Webhook:**  
  An incoming webhook URL for your Slack channel, saved as a repository secret called `SLACK_WEBHOOK_URL`.

- **(For advanced users/maintainers only):**  
  If you need to change how the workflow works, edit the files in `.github/workflows/`.

---

## Who is this for?

- Teams who want to stay updated on new images (or similar items) being completed in related repositories.
- Users who want a no-maintenance, no-server, automatic notification system.

---

## Need help?

If you have questions or need help setting this up, please ask your repository maintainer or open an issue in this repository.

---

## Scheduled Slack Message Deletion with GitHub Actions

This project includes a scheduled GitHub Action to automatically delete Slack bot messages older than 7 days.

### Workflow YAML
```yaml
name: Delete Old Slack Bot Messages

on:
  schedule:
    - cron: '0 2 * * *'  # Every day at 2am UTC
  workflow_dispatch:      # Allow manual trigger

jobs:
  delete-messages:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.x'
      - name: Install dependencies
        run: pip install requests
      - name: Delete old Slack bot messages
        env:
          SLACK_BOT_TOKEN: ${{ secrets.SLACK_BOT_TOKEN }}
          SLACK_CHANNEL_ID: ${{ secrets.SLACK_CHANNEL_ID }}
        run: python slack-app-code/delete_old_messages.py
```

### Usage
- Add your `SLACK_BOT_TOKEN` and `SLACK_CHANNEL_ID` as GitHub Secrets.
- The workflow will run daily or can be triggered manually from the Actions tab.
- No server or manual intervention is required.

---

**The Count** keeps counting so you don’t have to!
