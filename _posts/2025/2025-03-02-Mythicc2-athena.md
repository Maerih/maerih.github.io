---
layout: post
title: "Agent Athena: Mythic C2 Communications Over Discord"
date: 2024-01-10 19:43:00 +0300
categories: [C&C]
tags: 
pin: false
description: Using Discord bot as a covert channel, Mythic C2’s Athena enables stealthy communication for red team command and control.
toc: true
image: /assets/img/Athena/mainath.png
---

# What is Athena

- Athena is a fully-featured cross-platform agent designed using the crossplatform version of .NET (not to be confused with .Net Framework). Athena is designed for **Mythic 3.0** and newer.
- In this specific case, c2 communications are facilitated via a Discord Channel after the Agent starts sending back the victim’s data to the remote server.


# Installation

- To install Athena Agent for Mythic c2:
- [Athena Agent](https://github.com/MythicAgents/Athena)

- `./mythic-cli install github https://github.com/MythicAgents/Athena`

- Once installed it will appear in the **Installed Services** Section in Mythic

![Desktop View](/assets/img/Athena/1ath.png){: .shadow }

- Install also the [Discord](https://github.com/MythicC2Profiles/discord) profile for our communications.

`https://github.com/MythicC2Profiles/discord`

# Configuring Discord 

- We have to create a discord bot to facilitate our communitions.
- Before creating our bot we should create a discord server where this communication will be really tunneled through.

#  How to Create a Discord Server

Follow these steps to create your own Discord server:

---

##  Step 1: Sign In or Create a Discord Account

- Go to [Discord](https://discord.com)
- Log in or click **"Register"** to create a new account.

---

##  Step 2: Create a New Server

1. Open the Discord app (web/desktop/mobile).
2. On the left sidebar, click the **"+" (Add a Server)** button.
3. Select **"Create My Own"** or choose a template (e.g., Gaming, Study Group).
4. Choose:
   - **For Me and My Friends** *(or)*  
   - **For a Club or Community**

---

##  Step 3: Configure Server Basics

- **Server Name:** Give your server a name (e.g., `test`)
- **Server Icon:** (Optional) Upload an image/logo.
- Click **Create**.


![Desktop View](/assets/img/Athena/2ath.png){: .shadow } 
_Created a Discord Server to Facilitate our communications_


# Creating Discord Bot

- To create a discord bot navigate to : **developers console**

## 🔧 Step 1: Create a Discord Application

1. Go to the [Discord Developer Portal](https://discord.com/developers/applications)
2. Click **"New Application"**
3. Enter a name and click **"Create"**

##  Step 2: Create a Bot User

1. Inside your application, go to **> Bot** **Permission section** choose Administrator.
![Desktop View](/assets/img/Athena/3ath.png){: .shadow } 
_Setting up Conf_

2. On same **Bot** page Click the **reset token** and copy the **token**.
> Make Sure to Copy the Token after Reset.  
{: .prompt-danger }

3. Move to the **> OAuth2** page of the application setting.
- In the **Scopes Section** choose **bot**.
![Desktop View](/assets/img/Athena/4ath.png){: .shadow } 
_Setting up Conf_

- In the **Permission Section** choose **administator**. 
![Desktop View](/assets/img/Athena/5ath.png){: .shadow } 
_Setting up Conf_

- On bottom page you will be provided URL which will be used to integrate our bot in the server.Navigate to it.
![Desktop View](/assets/img/Athena/6ath.png){: .shadow } 
_Url to navigate to_


4. Once navigate choose your discord server you created earlier to put your bot application there.Also authorize the bot an administator.
![Desktop View](/assets/img/Athena/7ath.png){: .shadow } 
_Authorizing application to our server_

5. Vyoilla!! First part done.
![Desktop View](/assets/img/Athena/8ath.png){: .shadow } 
_confirming our bot got into server_

> Store the Server Id and Channel Id somewhere.we'll use them.  
{: .prompt-info }
# Generating Mythic- (Athena) Payloads

- Earlier we installed Athena agent.

## 🔧 Step 1: Generating Athena Payload

1. Navigate to `generate payload` tab in Mythic.
2. Os choose: `windows`
3. Payload choose: `Athena`
4. Choose command to include; for us we will only choose `whoami` and `exit` for testing purposes.

> Choose more commands for your operations.For demo purposes we will only include `whoami` and `exit`.  
{: .prompt-tip }

5. Profile choose: `Discord` 
- Here is where we input our **creds**: `Bot Token, Bot Channel Id`
![Desktop View](/assets/img/Athena/9ath.png){: .shadow } 
_Configuring Discord Token_

![Desktop View](/assets/img/Athena/10ath.png){: .shadow } 
_Configuring Channel Id_

6. **Next** -> **Create Payload**



7. **Edit config file in Mythic C2 Profile**
- Edit the `config.json` in the discord profile settings to include our creds.Token and channel Id.

![Desktop View](/assets/img/Athena/14ath.png){: .shadow } 
_Configuring Profile config.json_


- Restart the profile to take changes.

# Sending Payload to Victim.

- There are many ways to get your payload to the victim.ie phishing which is common.
- For us we'll just spin a python small webserver to host our payload for the victim.

`python3 -m http.server 9999`


> Defender should be disabled. We've **not** implemented any kind of evasion, and the Athena agent will  being flagged by Defender or most EDRs. 
{: .prompt-tip }

## Recieving Callback.

- Once our payload is executed on the victim we recieve a callback.But take note in our Discord server.Here is where our communications are being facilated on.

![Desktop View](/assets/img/Athena/11ath.png){: .shadow } 
_Getting Callback in Mythic_

### Back to our callback
- We get a callback.Lets interact with the victim.We only installed a few commands including the `whoami`.

![Desktop View](/assets/img/Athena/12ath.png){: .shadow } 
_runnig whoami command_

### On Discord
- Tasking our agent reaches to discord which is used as a middle man.We can see the conversation transaction in the Discord server we created earlier. 

![Desktop View](/assets/img/Athena/13ath.png){: .shadow } 
_unecrypted discord facilitator_



> This is just a POC.From here create a good Loader or Encrypt your payloads to avoid detection from EDRs and AVs. 
{: .prompt-danger }
