# Race-Control & Player-Flagsystem for Le Mans Ultimate 🏎️🏁

These companion desktop apps form a custom, cloud-connected race control system for Le Mans Ultimate (LMU).

**Race-Control** acts as the director's command center to manage flags, speed limits, and telemetry. **Player-Flagsystem** is the driver-facing dashboard that tracks live telemetry, enforces rules, and provides real-time audio and visual alerts. Together, they bridge the gap between a race director and the grid using a secure MQTT broker.

---

### The Story Behind the Software

Hi, I’m Rik vd Kruk, Co-Founder of RehnSports Racing and the creator of this application suite.

The idea for this software was born out of a desire to make sim racing feel as authentic as possible. When RehnSports Racing began planning a massive Endurance Series, we realized we desperately needed a reliable, immersive way to communicate live track conditions and safety car procedures directly to our drivers. We searched for a solution, but there simply wasn't any software out there that did exactly what we needed.

So, I decided to build our own in-house architecture from scratch.

It took a long time and a lot of trial and error to get the telemetry parsing, network syncing, and UI states perfectly aligned. But after countless hours of development and testing, we have finally arrived at the highly stable builds you see today: **Race-Control V1.8.1** and **Player-Flagsystem V2.8.1**.

*Why the jump in version numbers?* I bypassed standard semantic versioning (like v1.0.0) for the initial public release because the early development process was full of bugs and stability hurdles. I deliberately delayed uploading this to GitHub until all major problems were ironed out and the system actually worked reliably under heavy race conditions.

Once we saw how much this system elevated our own league, we knew we couldn't just keep it to ourselves. I strongly believe in not gatekeeping good tools, which is why I've open it up as release for the wider sim racing community.

#### Need Help or Support?

If you run into bugs, have questions, or want to share feedback, I am always happy to help:

* **Discord (Fastest):** Direct message me at `rikuvanderkruk`
* **Email:** Reach out at `rehnsportsracing123@gmail.com`

> **Note on Discord Integration:** This repository hosts the standalone versions, designed to run independently without a Discord webhook. If you want a specialized build that automatically pushes flag changes and race control events to your own Discord channel, reach out to me via the contacts above.

---

## 🏁 LMU Race Control Hub: Director Manual

This application serves as the centralized digital command center for Race Directors. It allows you to monitor live driver telemetry, mutate global track states, enforce speed limits, and manage Safety Car procedures.

### 1. Initial Setup & Connection

Upon launching the `.exe`, you will pass through a startup splash screen to the authentication module:

* **Room Code:** Enter a unique string identifier for your session (e.g., `LEMANS01` or `ENDURANCE_LEAGUE`).
* **Password:** Set a secure password for the room.

*How it works: The app establishes a secure MQTT cloud session. Drivers must enter these exact credentials in their client app to connect. Failed authentications are blocked and printed to your admin logs.*

### 2. Global State & Flag Commands

These controls mutate the global track state and automatically push speed limit updates to all connected driver clients.

| Flag Command | Effect |
| --- | --- |
| 🟩 **Green Flag** | Clears all restrictions. Racing logic resumes, and speed limits are lifted. |
| 🟨 **Safety Car (SC)** | Enforces a strict 60 km/h global limit as drivers form up behind the SC. |
| 🟨 **VSC (60 km/h)** | Deploys a Virtual Safety Car, pushing a track-wide 60 km/h limit. |
| ⏱️ **VSC Ending (10s)** | Initiates a 10-second countdown event, automatically throwing the Green Flag when complete. |
| 🟥 **Red Flag** | Suspends the session entirely. Routes drivers to the pits under a strict 60 km/h limit. |

### 3. Safety Car (SC) Procedures

When an SC is deployed, you have granular control over the pace logic:

* **SC Presets (125 / 150 km/h):** Instantly pushes a static speed limit to the grid.
* **SC: Free:** Lifts the SC speed limit entirely.
* **Lock Pace:** Grabs the current live telemetry speed of the designated SC driver and locks the field to that exact variable.
* **SC Auto-Sync:** When toggled **ON** (Green), the background loop constantly reads the SC's live speed and dynamically updates the field's speed limit to match it. *(Note: Intended for specific pacing scenarios only).*
* **Safety Car In This Lap:** Broadcasts to the grid that the SC is pitting, transferring pace control to the race leader.
* **Auto SC Pit Detect:** An automated listener. When **ON**, it detects the SC entering the pit lane and fires the "Safety Car In" protocol automatically.

### 4. Driver Queue & Wave-Arounds

* **Safety Car Line (Left):** Array of drivers currently restricted by the active speed limit.
* **Waved Through (Right):** Array of drivers released to overtake the SC.

**To execute a wave-around:** Select lapped drivers in the *Safety Car Line* and click **Release ->** to lift their limits. Once they catch the pack, select them in the *Waved Through* list and click **<- Recall** to reinstate restrictions.

**Automated Stewarding (Color Codes):**

* 🟢 **Green (Safe):** Driving at or below the active limit.
* 🟠 **Orange (Warning):** Driving up to 10 km/h over the limit.
* 🔴 **Red (Violation):** Driving >10 km/h over the limit (triggers automated penalty logging).

---

## 🏎️ Player Flagsystem Dashboard: Driver Manual

This client-side app is your in-car digital race engineer. Running alongside LMU, it parses shared memory telemetry to provide real-time flag updates, speed restrictions, and audio alerts directly from Race Control.

### 1. Connection & Authentication

Launch the `.exe` and input the credentials provided by your Director:

* **In-game Name:** Your exact racing/team name.
* **Room Code & Password:** Must exactly match the Director's active session.

**Connection Status (Center UI Light):**

* 🔴 **Red:** Disconnected / Auth Failed.
* 🟡 **Yellow:** Establishing cloud connection...
* 🟢 **Green:** Connected successfully (accompanied by a TTS voice confirmation).

### 2. HUD & Overlay Controls

The dashboard is designed for high-speed readability:

* **FLAG:** Current track state (e.g., `FLAG: GREEN`, `FLAG: VSC`).
* **TARGET:** Your required speed. `TARGET: FREE` means push. `TARGET: 60 km/h` means strict enforcement.
* **Current Speed:** Your live telemetry speed pulled directly from LMU.
* **Countdown Timer:** Visual and audio sync for deploying/ending caution periods.

**Customizing your view:**

* 📌 **Overlay Mode:** Pins the app as "Always on Top" so it overlays your simulator. Use the slider to adjust background opacity.
* ⚙️ **Hide Controls:** Removes menus and buttons to declutter the UI. You can then resize the window to scale up the core telemetry text.

### 3. Automated Procedures & Penalties

* **Speed Violations:** Exceeding the Target Speed by >10 km/h triggers a flashing red warning, an audio penalty command, and instantly pushes a violation log to the Director.
* **Pit Lane Detection:** The client automatically detects pit entry and enforces a 60 km/h target, overriding Green track states.
* **Wave Arounds:** If released by Race Control, your HUD switches to `TARGET: FREE (WAVE AROUND)` until you are recalled.

### 🚨 Playing as the Safety Car

If you are driving the physical Safety Car, the app has a hidden mode for you.

* **Login as:** `Safety Car`, `SC`, or `Safety-Car`.
* **Pace Mode:** Instead of restricting your speed, your client becomes a broadcaster, streaming your live telemetry to the Director for Auto-Sync pacing. When released, you will receive a specialized TTS prompt to clear the track.

---

## 🔌 LMU Shared Memory Plugin Setup

To allow the Player Flagsystem to read live speed and pit telemetry, LMU must broadcast its data via a Shared Memory Plugin. *(If you use apps like CrewChief or SimHub, you likely already have this enabled!)*

**1. Download the Plugin**

* Go to the [rF2SharedMemoryMapPlugin Releases](https://github.com/TheIronWolfModding/rF2SharedMemoryMapPlugin/releases) page.
* Download and extract the latest `.zip`. Locate `rFactor2SharedMemoryMapPlugin64.dll`.

**2. Install to LMU**

* Navigate to your LMU Plugins directory (default: `C:\Program Files (x86)\Steam\steamapps\common\Le Mans Ultimate\Plugins`).
* Drop the `.dll` file into this folder.

**3. Enable in Configuration**

* Launch LMU to the main menu once, then close it (this generates the config entry).
* Navigate to: `C:\Program Files (x86)\Steam\steamapps\common\Le Mans Ultimate\UserData\player`.
* Open `CustomPluginVariables.json` in a text editor.
* Set the `" Enabled"` value to `1` for the plugin:

```json
"rFactor2SharedMemoryMapPlugin64.dll":{
    " Enabled":1,
    " Project":""
}

```

You're done! Launch the Player Flagsystem alongside LMU and it will instantly parse your telemetry.

---

## 📸 Screenshots
---
<img width="1254" height="1254" alt="RACE-CONTROL" src="https://github.com/user-attachments/assets/262d8db3-be36-4ec9-8010-7e0232c2eb94" />
<img width="1254" height="1254" alt="PLAYER-FLAGSSYSTEM" src="https://github.com/user-attachments/assets/2b2fc3c2-ce00-4434-9529-05cd04931410" />
<img width="412" height="480" alt="Screenshot 2026-08-02 085353" src="https://github.com/user-attachments/assets/aa7d4cd0-7e3e-4acf-b695-cb9c238c9b67" />
<img width="293" height="172" alt="Screenshot 2026-08-02 085321" src="https://github.com/user-attachments/assets/f4ae8579-5ed8-4b03-bc33-7400eb623aea" />
<img width="303" height="172" alt="Screenshot 2026-08-02 085308" src="https://github.com/user-attachments/assets/f771a53c-ebff-4321-9cb6-c78bb09ce6dc" />
<img width="295" height="171" alt="Screenshot 2026-08-02 085254" src="https://github.com/user-attachments/assets/922aae85-52ae-4de4-b6ef-b335e873810a" />
<img width="629" height="1000" alt="Screenshot 2026-08-02 083808" src="https://github.com/user-attachments/assets/8fd33c1b-6df8-46a9-a896-88ff5d16d085" />
<img width="373" height="193" alt="Screenshot 2026-08-02 083748" src="https://github.com/user-attachments/assets/ad7794c6-d198-4d12-a526-b04465808573" />
<img width="365" height="187" alt="Screenshot 2026-08-02 083726" src="https://github.com/user-attachments/assets/428a6942-67ef-4e16-945a-8ca783f7c071" />
