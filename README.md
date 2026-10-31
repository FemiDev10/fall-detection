# FallSense

Fall detection for older and unwell people, using the phone they already own. No smartwatch needed.

Built for **Hacktoberfest 2026: Sunderland AI Hack Day** (GDG Sunderland × n8n).

## What it does

1. **Detects** a fall on the phone: free fall → impact → posture change → stillness.
2. **Ignores a dropped phone.** A phone lying on the floor is perfectly still. A phone in a person's hand or pocket picks up breathing and small movements. Sharp impacts, several bounces and a perfectly clean free fall also point to a drop.
3. **Scores severity from 0 to 100** using impact force, estimated fall height (h = ½gt²), posture change and how long the person stays down. The score keeps rising while they lie still.
4. **Asks "Are you okay?" out loud** and listens. "I'm okay" cancels the alert. "My hip hurts" sends it immediately.
5. **Alerts caregivers** through an n8n AI workflow, or straight from the phone if n8n can't be reached.
6. **Severe falls** open a call to the emergency number you set. The iPhone always asks before it dials.

## Run it (5 minutes)

The motion sensor only works over **HTTPS**.

1. Open the GitHub Pages link for this repo on the phone: `https://femidev10.github.io/fall-detection/`
   - **iPhone:** Safari → tap **Start monitoring** → **Allow** motion access.
   - **Android:** Chrome → tap **Start monitoring**. No prompt is needed.
2. Open **Contacts & settings**, then enter the patient's name, an ntfy topic (for example `fallsense-team7-x9`) and a teammate's number as the "number to call".
3. On the **caregiver phone** (the old Android works well), install the **ntfy** app and subscribe to the same topic. You can also open `https://ntfy.sh/<topic>` on a laptop.
4. Tap **Send test alert to caregivers**.

> **Never use 999 or 112 during the demo.** Use a teammate's number.

## n8n workflow (optional, for the Best n8n Automation award)

`n8n/fall-alert-workflow.json`: Webhook → AI triage agent (Claude or Gemini) → **Safety floor** (the AI can raise urgency but never lower it below the phone's tier) → ntfy push and optional Telegram → reply to the phone with the paramedic handover and a sentence to read aloud.

1. In n8n, go to **Workflows → Import from file** and choose the JSON file.
2. Open the chat model node and choose your Anthropic or Google Gemini credential and model. To use Gemini, swap the node for **Google Gemini Chat Model**.
3. Activate the workflow. Copy the **Production URL** from the webhook node and paste it into the app's **n8n webhook URL** setting.
4. (Optional) Enable the Telegram node and add a bot token and chat ID.

If n8n is down, the phone sends the ntfy alert itself.

## Demo script (3 min)

1. **Problem (20 s):** falls among older people are common, and lying on the floor for a long time is what turns them into emergencies. A smartwatch costs £300+, and most people already have a phone.
2. **Drop the phone on a cushion** → "Phone dropped, not a person." No alert.
3. **A teammate falls onto a mat** with the phone in their pocket → "Are you okay?" → "My hip hurts, I can't get up" → the caregiver's Android phone buzzes with the AI-written message → the paramedic handover appears on screen.
4. **Safety:** rules set the minimum, AI can only escalate, and a person confirms any emergency call.
5. **What's next:** a smartwatch app, dashboards for care homes, NHS / telecare integration, and a model trained on public fall datasets (SisFall, MobiAct).

As a backup, the **Demo: simulate a fall** buttons play sensor traces through the same detector, so no one has to fall.

## Tuning

Thresholds are in the `CFG` object at the top of the script in `index.html`. The **Last detection** card shows every measurement, so you can watch real values while you test.
