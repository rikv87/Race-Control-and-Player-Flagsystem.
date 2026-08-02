# The Story Behind the Software

Hi, I’m Rik vd Kruk, Co-Founder of RehnSports Racing and the creator of this application.

The idea for this software was born out of a desire to make sim racing feel as authentic as possible. When RehnSports Racing began planning a massive Endurance Series, we realized we desperately needed a reliable, immersive way to communicate live track conditions and safety car procedures directly to our drivers. We searched for a solution, but there simply wasn't any software out there that did exactly what we needed.

So, I decided to build our own in-house program from scratch.

It took a long time and a lot of trial and error to get the telemetry, networking, and UI perfectly synced. But after countless hours of development and testing, we have finally arrived at the highly stable builds you are using today: Race-Control V1.7.5 and Player-Flagsystem V2.7.5.

Once we saw how much this system elevated our own league, we knew we couldn't just keep it to ourselves. We strongly believe in not gatekeeping good tools, which is why we have released this software to the world for everyone in the sim racing community to use and enjoy.

Need Help or Support?
If you run into any problems, have questions, or want to share feedback, I am always happy to help:

   Discord: Send me a direct message (username): rikuvanderkruk (this is the fastest way to reach me).

   Email: You can also reach me at rikvdkruk@gmail.com.

Thank you for downloading, and I'll see you on the track!
--
# Race-Control-and-Player-Flagsystem.
These companion desktop apps form a cloud-connected race control system for Le Mans Ultimate. The first acts as the director's command center to manage flags, speed limits, and telemetry. The second is a driver dashboard that tracks telemetry, enforces rules, and provides real-time audio and visual alerts.
Overview of the Application

This project consists of two companion desktop applications designed to create a cloud-connected race control and telemetry system for Le Mans Ultimate (LMU). Together, they bridge the gap between a race director and drivers on the track by utilizing an MQTT broker for real-time cloud communication.

 Race Control Hub (Director App): Allows the session administrator to manage the race from a command center. Key features include deploying flags (Green, VSC, Safety Car, Red), setting and syncing speed limits, managing driver queues (such as the Safety Car line and waved-around drivers), and monitoring live telemetry and violations.

 Player Dashboard (Driver App): Runs alongside the simulator, connecting directly via shared memory to read vehicle telemetry. It provides real-time audio and visual alerts for flag statuses, enforces speed limits during caution periods, detects pit lane entries, and features an overlay mode for in-game visibility.

How to Use It

 Launch Race Control: Start the Race Control script. You will be prompted to enter a Room Code and a Password to establish a secure session on the cloud network.

 Launch the Driver Dashboard: Start the player dashboard script. Enter your Driver Name, the matching Room Code, and the Room Password set by the director.

 Connect & Race: Once authenticated, the dashboard connects to the cloud server. The race director can then trigger flags and speed constraints, which instantly transmit to all connected drivers with automated audio    announcements and visual indicators.

Why Versioning & GitHub Upload Timing Matter

The project starts at its current version number rather than a standard initial release (like v1.0.0) because the development process was filled with initial bugs, stability issues, and troubleshooting hurdles at the start. Uploading the code to GitHub was deliberately delayed until all major problems were ironed out and the system actually worked reliably.
Standalone Version & Custom Discord Integration

This specific version uploaded to GitHub is a standalone version designed to run independently without requiring a Discord webhook.

If you want a version integrated with Discord that sends automated flag messages directly into your own Discord channel, you can reach out via E-mail: rikvdkruk@gmail.com or Discord: rikuvanderkruk. A special customized version will be provided for those who want Discord notifications for race control events and flags.


# 🏁 LMU Race Control Hub: User Manual

Welcome to the **LMU Race Control Hub**! This application serves as a centralized digital command center for sim racing Race Directors. By running this executable, you can monitor live driver telemetry, deploy track-wide flags, enforce speed limits, and manage Safety Car procedures in real time.

Here is everything you need to know to get started and master the Race Control dashboard.

---

## 1. Initial Setup & Connection

When you launch the downloaded `.exe` file, you will be greeted by a brief startup splash screen, followed by two critical setup prompts:

* **Room Code:** Enter a unique identifier for your session (e.g., `LEMANS01` or `ENDURANCE_LEAGUE`).
* **Password:** Set a secure password for the room.

> **How it works:** The application connects to a cloud server. For drivers to appear on your dashboard, they must use their own client application and enter the exact same Room Code and Password you just created. If they enter the wrong password, their connection will be blocked, and you will see an "Auth Failed" message in your logs.

---

## 2. Dashboard Interface Overview

Once logged in, you will hear a voice prompt confirming your director status, and the main dashboard will open. The interface is broken down into several key command panels.

### Flag Command Buttons

These buttons dictate the global track state and automatically enforce speed limits on all connected drivers.

| Flag Command | Effect |
| --- | --- |
| 🟩 **Green Flag** | Clears all restrictions. Racing resumes, and speed limits are lifted. |
| 🟨 **Safety Car (SC)** | Enforces a strict 60 km/h global speed limit as drivers form up behind the SC. |
| 🟨 **VSC (60 km/h)** | Deploys a Virtual Safety Car, enforcing a track-wide 60 km/h limit. |
| ⏱️ **VSC Ending (10s)** | Initiates a 10-second countdown, automatically throwing the Green Flag afterward. |
| 🟥 **Red Flag** | Suspends the session. Enforces a 60 km/h speed limit for drivers returning to pits. |

### Safety Car Speed Limits & Procedures

When a Safety Car is deployed, you control the pace of the queue.

* **SC: 125 km/h** / **SC: 150 km/h:** Instantly sets the global Safety Car speed limit to these preset values.
* **SC: Free:** Lifts the Safety Car speed limit entirely.
* **Lock Pace:** Reads the current live telemetry speed of the driver logged in as the "Safety Car" and instantly locks the rest of the field to that exact speed.
* **Limit: 60 km/h** / **Limit: Lifted:** Quick toggles for custom lower limits or free pacing.
* **SC Auto-Sync:** When toggled **ON** (Green), the system constantly reads the live speed of the Safety Car and dynamically updates the speed limit for the rest of the field in the background. *(Note: Do Not Use in normal procedures)*.
* **Safety Car In This Lap:** Alerts all drivers that the SC is pitting and transfers pace control to the race leader.
* **Auto SC Pit Detect:** When toggled **ON**, the system will automatically detect when the Safety Car enters the pit lane and trigger the "Safety Car In" protocol without you needing to press the button.

### Driver Management (The Wave-Around System)

This section monitors all connected drivers and their live telemetry speeds.

* **Safety Car Line (Left List):** Drivers currently restricted by the active speed limit.
* **Waved Through (Right List):** Drivers who have been released to overtake the Safety Car and regain their lap.

**Managing Drivers:**

1. Select one or more drivers in the *Safety Car Line*.
2. Click **Release ->** to wave them through. Their speed limits will be instantly lifted so they can catch the back of the pack.
3. If a driver needs to be put back under restrictions, select them in the *Waved Through* list and click **<- Recall (Enforce Limit)**.

**Speed Violation Colors:**
Driver names in the lists are color-coded based on their live telemetry to help you spot penalties easily:

| Color Code | Status | Definition |
| --- | --- | --- |
| 🟢 **Green** | Safe | Driving safely at or below the active speed limit. |
| 🟠 **Orange** | Warning | Driving up to 10 km/h over the speed limit. |
| 🔴 **Red** | Violation | Driving more than 10 km/h over the limit. |

### Live Logs & Telemetry

At the bottom of the dashboard, three console windows provide real-time data:

* **Live Logs & Violations:** Shows driver join/leave events, authentication successes/failures, and automatic penalty warnings.
* **Background Actions:** A silent log showing the micro-adjustments being made (like auto-syncing speeds) without cluttering your main log.
* **Live Safety Car Telemetry:** Displays a dedicated, real-time feed of the Safety Car's exact speed on track.

---

## 3. Example Scenario: Deploying a Safety Car

To give you an idea of how this flows during a live race, here is a standard procedure:

1. **Incident Occurs:** You click **Safety Car**. The system alerts all drivers and enforces a 60 km/h limit to safely bunch the field.
2. **Forming Up:** The designated Safety Car driver logs into the system. You toggle **SC Auto-Sync: ON**. Now, whatever speed the Safety Car drives, the rest of the field is electronically restricted to match it.
3. **Lapped Cars:** You notice a lapped driver in the queue. You select their name in the *Safety Car Line* and click **Release ->**. They are freed from the speed limit to drive around the track and catch the back of the pack. Once they catch up, you select their name and click **<- Recall** to restrict them again.
4. **Going Green:** The track is clear. You click **Safety Car In This Lap**. Drivers are notified. Once the SC pulls away, you click **Green Flag** to release the field.

---

## 4. Important Notes for Users

* 🔊 **Audio Cues:** The app uses Windows Text-to-Speech and radio click sound effects to notify you of critical events (like drivers being recalled). Make sure your speakers or headset are unmuted.
* 🏎️ **Safety Car Recognition:** For the system to properly recognize the Safety Car driver (and prevent duplicates), they must connect using a name like `Safety Car`, `SC`, or `Safety-Car`.
* ❌ **Closing the App:** When you are done with the session, simply close the window. The app will disconnect from the cloud server and bid you farewell.


# 🏁 Player Flagsystem Dashboard: User Manual

This application is designed to be your in-car digital race engineer and flag display. By running this executable alongside your sim racing title, you will receive real-time flag updates, speed restrictions, and audio alerts directly from your Race Director.

---

## 1. Initial Setup & Login

When you launch the `.exe` file, you will see a brief startup screen followed by three login prompts. You must get the room details from your Race Director before joining.

* **In-game Name:** Enter your exact racing name or team name. *(Note: If you are driving the actual Safety Car, you must enter `Safety Car` or `SC` here—more details below).*
* **Room Code:** Enter the exact Room Code provided by your Race Director (e.g., `LEMANS01`).
* **Room Password:** Enter the password for the room.

### Connection Status

Once the main dashboard opens, look at the **Cloud Connection** light in the middle of the app:

| Status Light | Meaning |
| --- | --- |
| 🔴 **Red** | Disconnected or incorrect password. (A "Retry Connection" button will appear). |
| 🟡 **Yellow** | Connecting to the cloud... |
| 🟢 **Green** | Connected and ready to race! You will hear a voice confirm your connection. |

---

## 2. Understanding the Dashboard

The interface is designed to be highly visible while you are driving. It displays three main pieces of information:

* **FLAG (Top):** Shows the current track state (e.g., `FLAG: GREEN`, `FLAG: SC`, `FLAG: VSC`, `FLAG: RED`).
* **TARGET (Middle):** Shows the speed you are required to drive. `TARGET: FREE` means you can race at full speed. A number (e.g., `TARGET: 60 km/h`) means you are under a strict speed limit.
* **Current Speed (Bottom):** Reads live telemetry directly from your game to show how fast you are currently going.
* **Countdown Timer:** When a Safety Car or VSC is deployed, a countdown timer will appear here (with audio cues) giving you time to slow down before the speed limit is strictly enforced.

---

## 3. Key Features & Overlay Controls

Because you need to see this dashboard while playing your game, the app includes special viewing modes.

### 📌 Overlay Mode

Click the **"Enable Overlay Mode"** button to pin the application on top of your game window.

* This makes the app "Always on Top" so it won't disappear behind your game.
* **Opacity Slider:** Use the slider that appears to make the window transparent, allowing you to see the track behind the dashboard.

### ⚙️ Hide Controls (Bigger Telemetry)

Once you have set your opacity and enabled the overlay, click **"Hide Controls"**. This removes the buttons and sliders, allowing you to shrink the window down so it only displays the vital Flag and Target speed information.

> **Pro Tip:** The text automatically scales! You can click and drag the edges of the window to make the text as big or small as you need it to be on your screen.

---

## 4. In-Game Procedures & Penalties

The application reads live telemetry from your car. Here is how it behaves during different race scenarios:

* **Safety Car / VSC Deployment:** When the Race Director throws a yellow flag, a countdown will begin. You must reduce your speed to the Target Speed before the countdown hits zero.
* **Speed Violations:** If you exceed the Target Speed by more than 10 km/h once the limit is enforced, your dashboard will flash a red penalty warning, you will receive an audio penalty command, and an alert will be sent instantly to the Race Director.
* **Pit Lane Detection:** The app automatically knows when you enter the pit lane. It will enforce a 60 km/h target speed regardless of whether the track is Green or Yellow. Speeding in the pits will also trigger a warning.
* **Wave Arounds:** If the Director grants you a wave-around to pass the Safety Car, your dashboard will update to `TARGET: FREE (WAVE AROUND)`. You are allowed to speed up until the Director recalls you to the queue.

---

## 5. 🚨 Special Instructions: Playing as the "Safety Car"

If your job for the session is to drive the physical Safety Car on track, this app has a hidden mode just for you.

* **How to activate:** When logging in, type `Safety Car`, `SC`, or `Safety-Car` as your In-game Name.
* **What it does:** Your app will operate in **"Pace Mode"**. Instead of restricting your speed, your app will constantly stream your live telemetry directly to the Race Director.
* **Auto-Sync:** This allows the Race Director to lock the rest of the racing field to your exact live speed. When the Director releases you, you will hear a specialized voice line telling you to clear the track and return to the pits.
