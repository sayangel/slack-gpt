# Slack ChatGPT Bot on Heroku
_This document was written by ChatGPT and directed by Aaron Ng ([@localghost](https://twitter.com/localghost))._

_Original repo: [Slack ChatGPT bot](https://github.com/aaronn/slack-gpt)_

_Modified by Angel Say ([@sayangel](https://twitter.com/sayangel)) to be easily deployed on Heroku._

## Introduction

This script creates a Slack bot that uses ChatGPT to respond to direct messages and mentions in a Slack workspace. It functions as a general question-answering bot for your company.

## Environment Variables

### Required:

1. `OPENAI_API_KEY`: Your OpenAI API key, which starts with "sk-".
2. `SLACK_APP_TOKEN`: Your Slack App Token, which starts with "xapp-".
3. `SLACK_BOT_TOKEN`: Your Slack Bot Token, which starts with "xoxb-".

### Optional:

1. `MODEL`: The OpenAI model to use. Can be "gpt-3.5-turbo" or "gpt-4". Default is "gpt-3.5-turbo".
2. `PROMPT`: A custom prompt for the bot. Default is a predefined prompt for a friendly company assistant.

## Setup

1. Go to [https://api.slack.com/apps?new_app=1](https://api.slack.com/apps?new_app=1).
2. Click "Create New App".
3. Click "Basic", then name your Slack bot and select a workspace.

### Configuration

1. In "Settings" → "Socket Mode", enable both Socket Mode and Event Subscriptions.
2. In "Settings" → "Basic Information", install your app to the workspace by following the instructions.
3. In "Settings" → "Basic Information", scroll to "App-Level Tokens" and create one with the permission `connections:write`. Set the resulting token that starts with `xapp-` as your `SLACK_APP_TOKEN`.
4. In "Features" → "OAuth and Permissions", copy the "Bot User OAuth Token" and set it as the `SLACK_BOT_TOKEN` in your environment.
5. In "Features" → "OAuth and Permissions" → "Scopes", add the following permissions: `app_mentions:read`, `channels:history`, `channels:read`, `chat:write`, `chat:write.public`, `groups:history`, `groups:read`, `im:history`, `im:read`, `mpim:history`, `mpim:read`, `users:read`.
6. In "Features" → "Event Subscriptions" → "Subscribe to Bot Events", add the following bot user events: `app_mentions:read`, `message.im`.
7. In "Features" → "App Home", turn on the "Messages Tab" switch, and enable the `Allow users to send Slash commands and messages from the messages tab` feature.

Now your Slack bot should be ready to use!

## Deployment

### Heroku Deployment:

This version of the repo is adapted to be easily deployed to Heroku using a Python Poetry Buildpack. This assumes you have Heroku CLI tooling installed.

1. Set up a new Heroku app: `heroku create`.
2. Add the [Python Poetry Buildpack](https://elements.heroku.com/buildpacks/moneymeets/python-poetry-buildpack): 
    ```
    heroku buildpacks:clear
    heroku buildpacks:add https://github.com/moneymeets/python-poetry-buildpack.git
    heroku buildpacks:add heroku/python
    ```
    **Note**: If you have multiple Heroku apps deployed append `--app YOUR_APP_NAME` to each command before running it.

3. Set the python environmnet compatible with the buildpack: `heroku config:set PYTHON_RUNTIME_VERSION=3.9.16`
4. Configure all the environment variables from the [instructions above](#environment-variables) using `heroku config:set`
5. Deploy the app: `git push heroku main`
6. Add a dyno resource: `heroku ps:scale bot=1`

### Local Deployment:

1. If running locally, install dependencies with `poetry`.
2. Comment out these two lines in the script:

```
# from dotenv import load_dotenv
# load_dotenv()
```

Start the bot and enjoy using it in your Slack workspace.
