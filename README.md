# Race-Control & Player-Flagsystem for Le Mans Ultimate 🏎️🏁

These companion desktop apps form a custom, cloud-connected race control system for **Le Mans Ultimate (LMU)**.

**Race Control** is the director's command center for flags, speed limits, Safety Car procedures and live telemetry. **Player Flagsystem** is the driver-facing dashboard with an always-on-top LED overlay that shows the race status, counts down to every caution, enforces speed limits and gives real-time voice and sound alerts. Together they connect the race director and the grid through a secure MQTT cloud connection.

| App | Current version | For |
|---|---|---|
| 🏁 **Race Control** | **V2.5.1** | Race Director |
| 🏎️ **Player Flagsystem** | **V4.5.1** | Every driver (and the Safety Car) |

> ⚠️ **Always use matching versions.** Race Control V2.5.1 only works with Player Flagsystem V4.5.1. Older versions (Race Control V1.8.1 / Player Dashboard V2.8.1) cannot join a V2.5.1 room.

👉 **[Download the latest versions from the Releases page](https://github.com/rikv87/Race-Control-and-Player-Flagsystem./releases)**

---

## 📑 Contents

- [The Story Behind the Software](#the-story-behind-the-software)
- [What's New](#-whats-new-in-race-control-v251--player-flagsystem-v451)
- [Quick Start](#-quick-start)
- [Race Control: Director Manual](#-race-control-v251-director-manual)
- [Player Flagsystem: Driver Manual](#️-player-flagsystem-v451-driver-manual)
- [Playing as the Safety Car](#-playing-as-the-safety-car)
- [LMU Shared Memory Plugin Setup](#-lmu-shared-memory-plugin-setup)
- [Troubleshooting](#-troubleshooting)
- [Building From Source](#️-building-from-source)
- [Need Help or Support?](#need-help-or-support)

---

## The Story Behind the Software

Hi, I'm **Rik vd Kruk**, Co-Founder of **RehnSports Racing** and the creator of this application suite.

The idea for this software was born out of a desire to make sim racing feel as authentic as possible. When RehnSports Racing began planning a massive Endurance Series, we realized we desperately needed a reliable, immersive way to communicate live track conditions and safety car procedures directly to our drivers. We searched for a solution, but there simply wasn't any software out there that did exactly what we needed.

So, I decided to build our own in-house architecture from scratch.

It took a long time and a lot of trial and error to get the telemetry parsing, network syncing, and UI states perfectly aligned. The first public builds were **Race Control V1.8.1** and **Player Flagsystem V2.8.1**. The current generation, **Race Control V2.5.1** and **Player Flagsystem V4.5.1**, brings a WEC/IMSA-style **Full Course Yellow** system, a synchronised countdown for the whole grid, a new LED overlay and a complete redesign.

**Why the jump in version numbers?** I bypassed standard semantic versioning (like v1.0.0) for the initial public release because the early development process was full of bugs and stability hurdles. I deliberately delayed uploading this to GitHub until all major problems were ironed out and the system actually worked reliably under heavy race conditions.

Once we saw how much this system elevated our own league, we knew we couldn't just keep it to ourselves. I strongly believe in not gatekeeping good tools, which is why I've opened it up as a release for the wider sim racing community.

---

## ✨ What's New in Race Control V2.5.1 / Player Flagsystem V4.5.1

- 🟨 **Full Course Yellow replaces the Virtual Safety Car (VSC)**, with one clear sequence: announcement → countdown → enforced → ending → green
- ⏱️ **The countdown starts once the announcement has finished playing** on every driver's PC, not when the button is pressed
- 🔄 **One synchronised countdown for the whole grid** — every driver reaches zero at the same moment
- 🔊 **Two distinct sounds:** a loud klaxon when FCY is enforced, a bright chime when green comes out — no beeps every second
- 📺 **New always-on-top LED overlay** for drivers: movable, resizable, with a detachable countdown
- 🚗 **Safety Car in this lap** now gives drivers a grace period (default 5 s) to slow down
- ⚙️ **Procedure timing panel** in Race Control: change speeds and countdown lengths without touching code
- 💬 **Optional Discord alerts** built into Race Control
- 🗣️ **More reliable voice announcements** with automatic fallback to other Windows speech engines
- 🎨 **Completely redesigned interface** for both apps, inspired by the RehnSports Racing style

Full details are in the [release notes](https://github.com/rikv87/Race-Control-and-Player-Flagsystem./releases).

---

## 🚀 Quick Start

**Race Director**
1. Download and run `Race_Control_V2.5.1.exe`
2. Enter a **room code** and **password** and share them with your drivers
3. Wait for drivers to appear in the driver list, then run your session

**Drivers**
1. Install the [LMU Shared Memory Plugin](#-lmu-shared-memory-plugin-setup) (one time)
2. Download and run `Player_Flagsystem_V4.5.1.exe`
3. Enter your **in-game name**, the **room code** and the **password**
4. Press **Test voice + sounds** in the Session panel
5. Set LMU to **borderless / windowed** mode and position the LED overlay where you can see it

No Python installation is needed for the `.exe` files.

---

## 🏁 Race Control V2.5.1: Director Manual

Race Control is the centralized digital command center for Race Directors. It decides every flag and every countdown for the whole grid, monitors live driver telemetry, enforces speed limits and manages Safety Car procedures.

### 1. Initial Setup & Connection

After the splash screen you'll see the **Race Control Setup** window:

- **Room code:** a unique identifier for your session (e.g. `LEMANS01` or `ENDURANCE_LEAGUE`)
- **Room password:** a password drivers must enter to join

The app opens a cloud session on the MQTT broker. Drivers must enter these exact credentials to connect. Failed logins are blocked and shown in the race log. The header shows the cloud connection status and how many drivers are online.

### 2. Status Board

The large board at the top always shows:

- The **current flag** in its flag colour (Green, Full Course Yellow, Safety Car, Red Flag)
- The **stage**: *Announcing to drivers · Countdown running · Enforced*
- A **live LED countdown**
- The **current limit** and the limit that applies at zero
- During an announcement: **how many drivers have finished hearing it**, who is still listening, and the safety timeout

### 3. Flag Control

| Button | Effect |
|---|---|
| 🟨 **Deploy FCY** | Starts the Full Course Yellow sequence (see below). Default 80 km/h. |
| 🟨 **FCY ending → Green** | Announces the end of FCY, runs the ending countdown (default 10 s; the FCY limit stays enforced), then throws the Green Flag. Only available while FCY is enforced. |
| 🟧 **Safety Car** | Announcement → countdown (default 15 s) → 60 km/h enforced. Drivers chase the leader and form up behind the SC. |
| 🟧 **Safety Car in this lap** | Announcement → grace period (default 5 s) → SC-in limit (60 km/h). Green follows automatically when the Safety Car enters the pit lane. Only available under Safety Car. |
| 🟥 **Red Flag** | Suspends the session. Announcement → countdown → 60 km/h, drivers return to the pit lane. |
| 🟩 **Green Flag** | Clears all restrictions immediately. Racing resumes. |

### 4. How a Full Course Yellow Works

1. **You press Deploy FCY.**
2. **Every driver hears the FCY announcement** on their own PC (and on yours, if *play announcements on this PC* is on).
3. **Each driver's app reports when the announcement has completely finished.** When every connected driver has finished, Race Control starts **one** countdown. A safety timeout (max. 20 s) makes sure one frozen PC can never stall the race.
4. **The countdown runs** (default 15 s) on Race Control, every dashboard and every LED overlay. There are no beeps and no spoken numbers during it.
5. **At zero, FCY is enforced** (default 80 km/h) and the FCY warning sound plays on every PC at the same moment.
6. **FCY ending → Green:** announcement, ending countdown, then the Green Flag with its own sound.

Pressing another flag button at any point cancels the running sequence and starts the new one.

### 5. Safety Car Pace

Available while the Safety Car is out:

- **SC 125 km/h / SC 150 km/h:** push a fixed speed limit to the grid
- **SC free:** removes the numeric limit — the Safety Car dictates the pace, no overtaking
- **Lock SC pace:** takes the Safety Car's current live speed and sets it as the limit for the field
- **Live SC speed** and **SC pit status** (on track / in pit lane) are shown in real time

### 6. Speed Limit

- **Limit 60 km/h:** push a 60 km/h limit
- **Lift limit (free):** remove the speed limit

### 7. Procedure Timing

Everything can be adjusted without editing code, and is saved automatically:

| Setting | Default |
|---|---|
| FCY speed | 80 km/h |
| FCY countdown | 15 s |
| FCY ending countdown | 10 s |
| Safety Car countdown | 15 s |
| SC in this lap: grace period | 5 s |
| SC in this lap: limit | 60 km/h |
| Red Flag countdown | 15 s |
| Green after the Safety Car pits | 2.5 s |

**Play announcements and sounds on this PC:** when on, Race Control also plays every announcement and sound, and waits for its own announcement too.

### 8. Driver Queue & Wave-Arounds

- **Safety car line (left):** drivers currently restricted by the active limit
- **Waved through (right):** drivers released to overtake the Safety Car

To wave a driver around: select them in the Safety car line and click **Release (wave around) →**. Once they catch the pack, select them in Waved through and click **← Recall (enforce limit)**. Waved-around drivers are reset automatically when a new flag is deployed.

**Automated stewarding (colour codes):**

- 🟢 **Green (Safe):** at or below the active limit
- 🟡 **Yellow (Warning):** up to 10 km/h over the limit
- 🔴 **Red (Violation):** more than 10 km/h over the limit — a violation is logged
- ⚪ **Grey:** no telemetry received recently

### 9. Logs

- **Race log & violations:** joins, leaves, flag changes and speeding violations
- **Background:** every state change sent to the drivers
- **SC telemetry:** the Safety Car's live speed

The race log is also saved to `%APPDATA%\LMU-FCY-System\logs\RaceControl_Log_<ROOM>.txt` for post-race review.

### 10. Discord Alerts (optional)

Race Control can post flag changes (FCY, FCY ending, Safety Car, SC in this lap, SC free, Red, Green) and speeding violations to a Discord channel:

1. In Discord: **Server settings → Integrations → Webhooks → New Webhook → Copy URL**
2. In Race Control, paste the URL into the **Discord alerts** card
3. Press **Send test** — the card tells you if it arrived
4. Switch the toggle on

Discord alerts are **off by default**; the apps work fully without them.

---

## 🏎️ Player Flagsystem V4.5.1: Driver Manual

The Player Flagsystem is your in-car digital race engineer. Running alongside LMU, it reads your telemetry from shared memory and shows flags, speed limits, countdowns and warnings from Race Control, with voice announcements and alert sounds.

### 1. Connection & Login

Launch the `.exe` and enter the credentials from your Race Director:

- **In-game name:** your exact racing/team name
- **Room code & password:** must exactly match the Director's session

Your name and room are remembered for next time.

**Connection status (top right):**

- 🔴 **Offline:** disconnected or login failed (a **Retry connection** button appears)
- 🟡 **Connecting / Authenticating:** establishing the cloud connection
- 🟢 **Connected:** ready (with a voice confirmation)

If the connection drops, the app reconnects automatically. If Race Control goes offline, a warning banner appears.

### 2. The Dashboard

- **Status board:** the current flag in its colour (Green Flag, Full Course Yellow, Safety Car, Red Flag) and what is happening right now (e.g. *FCY imminent*, *FCY enforced*, *FCY ending · prepare for green*)
- **Countdown:** LED-style countdown to the next change (e.g. *FCY IN 12*, *GREEN IN 5*)
- **Target:** the speed you must respect — `NO LIMIT`, `80 KM/H`, `60 KM/H · PIT LANE`, `FREE · WAVE AROUND`, `FOLLOW SC · NO OVERTAKING`, or the limit that applies at zero
- **Your speed:** live from LMU, coloured 🟢 within the limit, 🟡 up to 10 km/h over, 🔴 more than 10 km/h over

**Toolbar:**

- **📌 On top:** keeps the dashboard itself above other windows (separate from the overlay)
- **Overlay ON/OFF:** show or hide the LED overlay
- **Hide controls:** removes the settings panels for a larger telemetry display. Resize the window and everything scales.

### 3. LED Status Overlay

A separate LED-style display that stays on top of your game, so you don't need the dashboard:

- Flag status (**FCY / GREEN / SC / RED**) in large flag colours
- Your target speed and what happens next
- Large LED countdown digits with a progress bar
- Flashes when a caution becomes enforced or green comes out

**Using it:**

- **Drag** anywhere on the overlay to move it
- **Drag the bottom-right corner** to resize it — everything scales
- **Right-click** for: show dashboard, always on top, detach countdown, reset position, hide overlay

**Overlay settings** (LED overlay panel on the dashboard):

| Setting | What it does |
|---|---|
| Show overlay | Show or hide the overlay |
| Overlay always on top | Keep it above your game |
| Detach countdown | Put the countdown in its own separate window, movable anywhere |
| Overlay opacity | 10–100% (**default 100%**) |
| Overlay size | Width of the overlay |
| Countdown size | Size of the countdown digits / detached countdown |
| Overlay position | Presets: top left, top centre, top right, bottom left, bottom centre, bottom right |
| Reset buttons | Return overlay and countdown to their default positions |

All settings are saved and restored when you restart the app.

> 💡 **Set LMU to borderless or windowed mode.** Exclusive full-screen games draw over every other window, including "always on top" ones.

### 4. What You Will Hear

| Situation | Audio |
|---|---|
| FCY deployed | Voice announcement; the countdown starts when it has finished |
| FCY enforced (zero) | 🟨 **FCY klaxon** (loud three-pulse sound) |
| FCY ending | Voice announcement, then the ending countdown |
| Green flag | 🟩 **Green chime**, then "Green flag" |
| Safety Car / Red Flag / SC in this lap | Voice announcement, then the enforcement sound at zero |
| Speed limit changes, SC free, wave-around, recall | Voice announcement |
| Speeding | Warning sound + voice warning |

There are no beeps every second and no spoken countdown numbers. Voice lines never talk over each other.

**Session panel:** shows which voice engine is in use (**Voice: …**) and has a **Test voice + sounds** button.

### 5. Automated Procedures & Penalties

- **Speed violations:** exceeding the target by more than 10 km/h triggers a warning banner, a warning sound and voice message, and sends a violation report to Race Control (max. one warning every 10 seconds)
- **Pit lane detection:** in the pit lane the target becomes 60 km/h automatically (violation above 64 km/h), regardless of the track state
- **Wave-arounds:** when released by Race Control your target shows `FREE · WAVE AROUND` until you are recalled

---

## 🚨 Playing as the Safety Car

If you are driving the Safety Car, the Player Flagsystem has a special mode:

- **Log in as:** `Safety Car`, `SC`, `Safety-Car` or `SafetyCar`. Only one Safety Car can be connected per room.
- **No speed checks:** your speed is never restricted or reported as a violation.
- **Telemetry broadcaster:** your live speed and pit-lane status are streamed to Race Control (used for **Lock SC pace** and the automatic green after **Safety Car in this lap**).
- **Your own announcements:** e.g. *"Pick up the overall race leader"*, a notice for every waved-around driver, and a prompt when you are released to dictate the pace.

---

## 🔌 LMU Shared Memory Plugin Setup

To let the Player Flagsystem read your live speed and pit status, LMU must broadcast its data via a Shared Memory Plugin. (If you use apps like CrewChief or SimHub, you likely already have this enabled!)

**1. Download the plugin**

Go to the **rF2SharedMemoryMapPlugin** releases page, download and extract the latest `.zip`, and locate `rFactor2SharedMemoryMapPlugin64.dll`.

**2. Install to LMU**

Copy the `.dll` into your LMU Plugins folder (default: `C:\Program Files (x86)\Steam\steamapps\common\Le Mans Ultimate\Plugins`).

**3. Enable in the configuration**

1. Launch LMU to the main menu once, then close it (this creates the config entry).
2. Go to `C:\Program Files (x86)\Steam\steamapps\common\Le Mans Ultimate\UserData\player`.
3. Open `CustomPluginVariables.json` in a text editor.
4. Set `" Enabled"` to `1` for the plugin:

```json
"rFactor2SharedMemoryMapPlugin64.dll":{
    " Enabled":1,
    " Project":""
}
```

You're done! Launch the Player Flagsystem alongside LMU and it reads your telemetry automatically. Without the plugin, the app still shows flags and countdowns, but your speed shows 0 km/h and speeding can't be detected.

---

## 🧰 Troubleshooting

**No voice announcements**
- Check the **Voice:** line in the Player Flagsystem Session panel and press **Test voice + sounds**
- Make sure a Windows voice is installed: *Settings → Time & language → Speech → Manage voices*
- Check the Windows volume mixer — the app can be muted separately from the game
- When running from source: `py -3 tools\voice_check.py` tests every speech engine

**"No Race Control found for this room"**
- Check the room code (not case-sensitive) and that Race Control is running
- Make sure both apps are the matching versions (Race Control V2.5.1 + Player Flagsystem V4.5.1)

**"Wrong room password"** — ask the Race Director for the password again.

**"The Safety Car role is already taken"** — another driver is already logged in as the Safety Car.

**Overlay disappears behind the game** — switch LMU to borderless / windowed mode and check that *Overlay always on top* is on.

**Speed always shows 0 km/h** — the [Shared Memory Plugin](#-lmu-shared-memory-plugin-setup) is not installed or not enabled.

**Where are settings and logs?** — `%APPDATA%\LMU-FCY-System\` (settings `.json` files and the `logs` folder, including error logs for the `.exe` versions).

**Custom sounds** — put a `.wav` with the same name (`fcy_enforced.wav`, `green_flag.wav`, `warning.wav`, `mic_click.wav`) in a `sounds` folder next to the `.exe`.

---

## 🛠️ Building From Source

Requirements: Windows and Python 3.10 or newer from [python.org](https://www.python.org/downloads/) (tick **Add python.exe to PATH**).

**Run from source**

```bat
py -3 -m pip install -r requirements.txt
py -3 race_control.py
py -3 player_dashboard.py
```

**Build the .exe files**

Double-click **`build.bat`**. It creates a build environment, installs the requirements, runs the tests and builds:

- `dist\Race_Control_V2.5.1.exe`
- `dist\Player_Flagsystem_V4.5.1.exe`

Everything is logged to `build_log.txt`.

- **App names and versions** are set in `fcy_common\__init__.py`
- **Icons:** `assets\icons\race_control.ico` and `assets\icons\player_flagsystem.ico`
- **Splash logos:** `assets\images\RACE-CONTROL.png` and `assets\images\PLAYER-FLAGSSYSTEM.png`

**Project structure**

| Path | Contents |
|---|---|
| `race_control.py` | Race Control app |
| `player_dashboard.py` | Player Flagsystem app |
| `fcy_common/` | Shared core: race-control state machine, protocol and clock sync, audio, overlay, UI kit, settings |
| `assets/` | Sounds, icons and logos |
| `tests/` | Automated tests (logic, audio, and both apps running together) |
| `tools/` | Sound generator and voice check |
| `fcy_build.spec` / `build.bat` | PyInstaller build |

---

## 📸 Screenshots

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/716d42e6-7d83-472f-b538-4c48ff4b820d" />
<img width="1181" height="1002" alt="image" src="https://github.com/user-attachments/assets/9cc2dd84-3503-441a-aced-04f657dc2fa6" />


---

## Need Help or Support?

If you run into bugs, have questions, or want to share feedback, I am always happy to help:

- **Discord (fastest):** direct message me at **rikuvanderkruk**
- **Email:** **rehnsportsracing123@gmail.com**

Bug reports are also welcome via [GitHub Issues](https://github.com/rikv87/Race-Control-and-Player-Flagsystem./issues). Please include your app versions and, if possible, the log files from `%APPDATA%\LMU-FCY-System\logs\`.

---

*Race Control V2.5.1 · Player Flagsystem V4.5.1 · Built by Rik vd Kruk for RehnSports Racing and the sim racing community.*
