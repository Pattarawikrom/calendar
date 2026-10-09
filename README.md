# Status Indicator

Status Indicator lets someone else, such as your partner, family or a colleague, see on their phone whether you are **free, in a meeting or in a call**. They also see your calendar for the coming days, as free/busy only.

- **Windows app** (`StatusIndicator.exe`): runs in the taskbar, detects calls from microphone use and reads your published calendar.
- **Web page** (`index.html`): the view for the other person. It can be added to the phone's home screen like an app.
- **No server or account needed**: data travels through the free notification service [ntfy.sh](https://ntfy.sh).
- **Privacy**: only times and free/busy are shared. Meeting titles, attendees and locations are never shared.

---

## Contents

1. [How it works](#1-how-it-works)
2. [Package contents](#2-package-contents)
3. [Requirements](#3-requirements)
4. [Install the Windows app](#4-install-the-windows-app)
5. [Publish the web page (GitHub Pages)](#5-publish-the-web-page-github-pages)
6. [Share with a viewer](#6-share-with-a-viewer)
7. [Using the app](#7-using-the-app)
8. [What the page shows](#8-what-the-page-shows)
9. [Settings](#9-settings)
10. [Privacy and security](#10-privacy-and-security)
11. [Files and locations](#11-files-and-locations)
12. [Updating](#12-updating)
13. [Uninstalling](#13-uninstalling)
14. [Troubleshooting](#14-troubleshooting)
15. [Technical details](#15-technical-details)

---

## 1. How it works

```
Your PC                          ntfy.sh                     Viewer's phone
───────                          ───────                     ──────────────
StatusIndicator.exe checks       "Mailbox" on the internet,  Web page (index.html)
every 30 seconds:                keeps the latest message    fetches the latest
 • Is a meeting app using the    for a few hours             message every 20 seconds
   microphone?             ───►                        ◄───  and displays it
 • Calendar (ICS link)
```

- The **app** detects calls when a meeting app (Teams, Zoom, Webex, …) or a remote desktop client uses your microphone. This also works while you are muted, because meeting apps keep the microphone open during a call. Your meetings come from your **published calendar** (an ICS link from Outlook, Google Calendar or any other calendar).
- **ntfy.sh** is a free notification service. The **channel name** is the address of your "mailbox".
- **index.html** contains no data. It only defines how the data is displayed. It must be reachable via a web address, for example free on GitHub Pages.

---

## 2. Package contents

| File | Purpose |
|---|---|
| `StatusIndicator.exe` | Windows app |
| `index.html` | Web page for viewers (upload to GitHub Pages or any static host) |
| `README.md` | This guide |

---

## 3. Requirements

- **Windows 10 or 11**. The required .NET Framework 4.8 is already part of Windows.
- A calendar that can be **published as an ICS link**, such as Outlook, Google Calendar, iCloud or Nextcloud.
- For the page: a free **GitHub account**, or any other static web hosting.
- Viewers only need a web browser.

---

## 4. Install the Windows app

### 4.1 Get your ICS link

**Outlook / Microsoft 365**

1. Open **Outlook on the web**.
2. Go to ⚙️ **Settings → Calendar → Shared calendars**.
3. Under **Publish a calendar**, choose your calendar and **Can view when I'm busy**, then click **Publish**.
4. Copy the **ICS link**. You don't need the HTML link.

**Google Calendar**

1. Open **Settings**, select your calendar on the left, then go to **Integrate calendar**.
2. Copy the **Secret address in iCal format**.

> Use a free/busy level if your calendar offers one. The app only shares times anyway, but whoever has the ICS link itself could see more.

### 4.2 Install

1. Unzip the package and double-click **StatusIndicator.exe**.
2. Windows may show **"Windows protected your PC"** because the app is not code-signed. Click **More info**, then **Run anyway**.
3. Answer **"Install Status Indicator now?"** with **Yes**. The app then:
   - copies itself to `%LOCALAPPDATA%\Programs\StatusIndicator\` (no admin rights needed),
   - adds a **Start menu** entry,
   - starts automatically with Windows.

   **No** runs it once without installing.
4. In the setup window:
   - **Your name**: shown to viewers.
   - **ICS link**: paste it and click **Test**. You should see "Works: … entries found".
   - **Channel name**: generated automatically.
   - **Page address**: add it later, once the page is online (step 5).
5. Click **Start**.

The icon appears at the bottom right of the taskbar. Windows 11 often hides new icons under the **^** arrow; drag the icon to the taskbar to keep it visible.

### 4.3 Check

Click the icon and choose **Check status …**:

- The meetings for the next 7 days should match your calendar.
- During a call, **"Call detected: yes"** should be shown.

---

## 5. Publish the web page (GitHub Pages)

You only do this once.

1. Sign up or sign in at [github.com](https://github.com).
2. Create a repository: **+ → New repository**, name it for example `status`, choose **Public**, tick **Add a README file**, then click **Create repository**.
3. Upload the page: **Add file → Upload files**, drop `index.html` in, then click **Commit changes**.
   - The file must be named exactly `index.html`, not `index (1).html`.
4. Turn on Pages: **Settings → Pages → Source: Deploy from a branch → Branch: main, / (root) → Save**.
5. After 1–2 minutes the page is live at `https://<username>.github.io/<repository>/`.
6. Enter this address in the app under **Settings → Page address**.

> The page contains no personal data. The channel name is only in the link after the `#`, and browsers never send that part to the server.

Any other static hosting works as well, for example Netlify, Cloudflare Pages or your own web space.

---

## 6. Share with a viewer

1. Click the icon and choose **Copy viewer link**. The link looks like this:
   ```
   https://<username>.github.io/<repository>/#status-xxxxxxxxxxxxxxxx
   ```
2. Send it, for example via a messenger.
3. The viewer opens it and adds it to the home screen:
   - **iPhone (Safari):** Share button → **Add to Home Screen**
   - **Android (Chrome):** **⋮** → **Add to Home screen**

Alternatively, the viewer opens the address without `#…` and types the channel name into the setup field. The page remembers it.

**Optional, push notifications:** install the **ntfy** app (App Store / Google Play, no account needed), tap **+** and subscribe to the **channel name**. The viewer then gets notifications such as "Alex is in a call" or "Alex is free again".

---

## 7. Using the app

Clicking the icon (left or right) opens the menu.

**Icon colors:**

| Icon | Meaning |
|---|---|
| 🟢 green | free |
| 🔴 red | in a call |
| 🟠 orange | in a meeting according to the calendar (no call detected) |
| 🟣 purple | away (calendar status "Out of office") |
| ⚪ grey with pause bars | paused, nothing is shared |
| ⚫ dark grey | app stopped, please restart |

**Menu:**

| Item | Function |
|---|---|
| *(top line)* | Current status as it is shared |
| **Pause / Resume** | Stop or resume sharing. While paused, neither status nor calendar is shared and viewers see "Paused". Survives restarts. |
| **Pause for 1 hour** | Resumes automatically afterwards |
| **I'm in a meeting** | Sets the status to "In a call" manually until you untick it. Fallback if a meeting isn't detected. |
| **Copy viewer link** | Complete link including the channel name (only shown once a page address is set) |
| **Copy channel name** | Only the channel name |
| **Check status …** | What is detected right now: calendar, microphone, call yes/no |
| **Settings …** | See [Settings](#9-settings) |
| **Open log** | Log file for troubleshooting |
| **Uninstall …** | Removes everything (see [Uninstalling](#13-uninstalling)) |
| **Quit** | Closes the app; viewers see "Paused". Start it again via the Start menu → **Status Indicator**. |

---

## 8. What the page shows

**Status card:**

| Display | Meaning |
|---|---|
| **In a call** (red, pulsing) | Call detected, with "since" and, if the calendar has a meeting, a progress bar with the time left |
| **In a meeting** (orange) | Busy according to the calendar, no call detected |
| **Free** (green) | Including how long until the next meeting |
| **Away** (purple) | Calendar status "Out of office" |
| **Paused** (grey) | Sharing paused or app closed; no calendar shown |
| **Computer offline** (grey) | No update for more than 45 minutes; the last known calendar is still shown |
| **No data yet** | No update for more than about 12 hours |

**Below the status card:**

- **Available today**: free time from now on (at least 15 minutes)
- **Week**: the next 7 days. The bar shows how full each day is, relative to 8 am–6 pm. Tap a day to see it.
- **Day view**: timeline with meetings and free slots (dashed green). The red line marks "now"; the current meeting fills up over time.

**Appearance (◐ at the top right):**

- **Automatic** (default): follows the phone's light or dark mode. If the browser can't tell, the page is dark.
- **Light** / **Dark**: fixed choice
- **Color** of meetings: blue, teal, rose or amber

The page adapts to any screen size (tested from 320 px wide phones to tablets). Dates are shown in the viewer's browser language.

---

## 9. Settings

| Setting | Meaning |
|---|---|
| **Your name** | Shown on the page and in push notifications. Defaults to the Windows user name. |
| **ICS link** | Your published calendar. **Test** checks it. |
| **Channel name** | Address of your mailbox on ntfy.sh. Allowed are letters a–z, digits, `-` and `_`, 8–55 characters. **Generate random channel name** creates a new one. After a change, the old link stops working, the old channel shows "Paused", and viewers need the new link. Short or simple names can be guessed, so use at least 16 characters. |
| **Page address** | Where `index.html` is hosted. Only used for **Copy viewer link**. |
| **Send push notifications** | Notifications for the ntfy app. Without them, only the page is updated. |
| **Count tentative meetings as busy** | Otherwise tentative meetings are only shown striped and don't change the status |
| **Microphone use via Remote Desktop counts as call** | On by default. For meetings on another computer that you control via Remote Desktop (RDP), Windows App, Citrix or VMware: the remote client then uses your local microphone. |
| **Microphone use in browsers counts as call** | For web meetings (e.g. Google Meet). Note: then **any** microphone use in a browser counts as a call. |
| **Start with Windows** | Autostart on or off |

**Meeting apps detected by default:** Microsoft Teams, Zoom, Webex, Slack, Discord, Skype, WhatsApp, Signal, GoTo and RingCentral. You can change the list in `config.json` under `CallApps` (wildcards `*`).

---

## 10. Privacy and security

**Shared:** your status (free / call / meeting / away / paused), the times of your meetings for the next 7 days with their free/busy kind, and your display name.

**Never shared:** meeting titles, attendees, locations, audio, or the ICS link itself.

**Things to know:**

- **The channel is not password-protected.** It is only protected by its random 16-character name, which is practically impossible to guess. Anyone who has the link can read along and could also post fake messages. The page only accepts data in the expected format and never runs code from messages. If a link has been passed on, use **Generate random channel name**.
- **Treat the ICS link like a password.** It stays on your PC only. If it leaks, unpublish and republish the calendar to get a new link.
- **ntfy.sh** keeps messages for about 12 hours.
- **Work calendars:** check your organisation's policy before sharing free/busy times of a work calendar with others.
- **The app itself** runs without admin rights. It only reads microphone usage from the Windows registry and the calendar, and changes nothing else on your system.

---

## 11. Files and locations

| What | Where |
|---|---|
| Program | `%LOCALAPPDATA%\Programs\StatusIndicator\StatusIndicator.exe` |
| Settings | `%APPDATA%\StatusIndicator\config.json` |
| Log | `%APPDATA%\StatusIndicator\status.log`. Cleared on every start and on quit. |
| Start menu entry | `%APPDATA%\Microsoft\Windows\Start Menu\Programs\Status Indicator.lnk` |
| Autostart | Registry `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`, value `StatusIndicator` |

Tip: you can type paths with `%…%` straight into the Explorer address bar.

---

## 12. Updating

1. Start the new `StatusIndicator.exe`.
2. If the old version is running, answer **"Stop the running instance?"** with **Yes**.
3. Answer **"Update Status Indicator?"** with **Yes**.

Your settings and channel name are kept. Earlier German versions ("Status-Anzeige") are detected and removed, and their settings are taken over.

If `index.html` has changed as well, upload it to your hosting again. On GitHub, open the file, click the pencil icon, replace the content, then click **Commit changes**.

---

## 13. Uninstalling

Click the icon, choose **Uninstall …** and confirm. This removes:

- autostart,
- the Start menu entry,
- settings and log,
- the program folder.

Viewers then see "Paused". Delete the GitHub repository yourself if you no longer need it (**Settings → Delete this repository**). Also unpublish the calendar in your calendar app.

---

## 14. Troubleshooting

**Windows blocks the app ("Windows protected your PC")**
→ Click **More info → Run anyway**. This only happens on the first start, because the app is not code-signed.

**The app runs in the background but there is no icon / I want to stop it**
→ Start Status Indicator again (Start menu). It asks "already running – stop the running instance?" → **Yes**.

**I can't see the icon**
→ Click the **^** next to the clock and drag the icon to the taskbar. Or go to **Settings → Personalization → Taskbar → Other system tray icons** and turn on Status Indicator.

**Test says "This is not a calendar link"**
→ You pasted the HTML link. Use the ICS / iCal link.

**Meetings are missing or outdated**
→ Calendar providers update published calendars with a delay, from minutes to a few hours. The app fetches the calendar every 5 minutes. **Check status …** shows what the ICS link currently contains.

**A call is not detected**
→ Open **Check status …** during a call and look at **"Apps using the microphone"**:
- A meeting app is listed but "Call detected: no"? Add its name to `CallApps` in `config.json`.
- A browser is listed? Turn on "Microphone use in browsers counts as call".
- A remote desktop client is listed (`mstsc.exe`, `msrdc.exe`, …)? Turn on "Microphone use via Remote Desktop counts as call". In the Remote Desktop connection, **Local Resources → Remote audio → Settings → Remote audio recording** must be "Record from this computer".
- Nothing is listed? Check **Windows Settings → Privacy & security → Microphone**.
- The log also records every change, for example "Microphone used by: mstsc.exe -> call".
- Meetings on another device with its own microphone, such as a phone, can't be detected. Use **I'm in a meeting** instead.

**Shown as "In a call" although I'm not in one**
→ **Check status …** shows which app is using the microphone. Some apps or remote clients keep it open. In that case, turn off the matching setting.

**Menu says "(connection problem)"**
→ ntfy.sh or the calendar can't be reached right now, for example because there is no internet or a VPN or firewall blocks it. The app keeps retrying. Details are in **Open log**.

**Page shows "No data yet" or "Computer offline"**
→ Check that the app is running and not paused. Also check that the channel name in the link (after `#`) matches the one in the app (**Copy channel name**).

**Page still looks old after an update**
→ GitHub Pages and browsers cache for up to 10 minutes. Reload (Ctrl + F5), or append `?v=2` to the address.

---

## 15. Technical details

| Parameter | Value |
|---|---|
| Microphone / status check | every 30 seconds |
| Calendar (ICS) refresh | every 5 minutes |
| Status change | taken after 2 identical checks (about 30–60 s) |
| Heartbeat to the page | on every change, otherwise every 20 minutes |
| Page refresh | every 20 seconds |
| "Computer offline" | after 45 minutes without an update |
| Calendar range | 7 days |
| Server | `https://ntfy.sh` |

**ntfy channels:**

- `<channel>`: readable push messages for the ntfy app, only on status changes
- `<channel>-data`: compact JSON snapshot for the page

**Call detection:** Windows records for each app in `HKCU\Software\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\microphone` when it uses the microphone. `LastUsedTimeStop = 0` means "in use right now". An app counts as a call if its name matches `CallApps`, or the remote desktop or browser lists when those settings are on.

**Calendar:** supports single and recurring meetings (daily, weekly, monthly, yearly, intervals, weekdays, "2nd Thursday" and so on), exceptions and moved occurrences, all-day events and time zones. Events marked free or working elsewhere, as well as cancelled events, are ignored.

**Page:** `index.html` is a single file without external dependencies. A Content Security Policy only allows connections to `ntfy.sh`. The channel name and appearance settings are stored locally in the browser.
