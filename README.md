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

If you want a version integrated with Discord that sends automated flag messages directly into your own Discord channel, you can reach out via E-mail: rikvdkruk@gmail.com. A special customized version will be provided for those who want Discord notifications for race control events and flags.
