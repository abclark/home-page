# Sharing iPhone GPS with OpenClaw on a Raspberry Pi

Five ways to get your location from your pocket to your agent. Each trades ease for control.

---

## 1. Telegram Live Location

**How:** Share live location in your Telegram chat with the bot. Telegram sends location updates for 1–8 hours. The bot receives them as message updates with latitude and longitude.

**Ease:** Trivial. Two taps in Telegram. No infrastructure needed.
**Reliability:** Updates arrive every 30–60 seconds. Telegram's servers handle delivery. Stops when the sharing window expires — you must re-share manually.
**Privacy:** Your coordinates pass through Telegram's servers. You trust Telegram already if you use it as your channel.

**Verdict:** Best quick option. Good enough for most uses. No code required.

## 2. iOS Shortcuts → API

**How:** Build a Shortcut that grabs the current location and POSTs it to an endpoint on your Pi (or a relay). Trigger it manually, via automation (time, arriving/leaving), or from the Share Sheet.

**Ease:** Moderate. Requires exposing an API or using a tunnel (Cloudflare, Tailscale). The Shortcut itself takes five minutes to build.
**Reliability:** Automations based on time or geofence work well. Background execution has limits — iOS may delay or skip triggers silently.
**Privacy:** Excellent. Data flows directly to your server. No third party sees it.

**Verdict:** Best for event-driven location (leaving home, arriving at work). Not suited for continuous tracking.

## 3. OpenClaw Node Pairing (location_get)

**How:** Install the OpenClaw companion app on the iPhone. Pair it as a node. The agent calls `location_get` on demand.

**Ease:** Simple once paired. The agent pulls location when it needs it — no polling, no sharing windows.
**Reliability:** Depends on the phone's network connection and the app staying alive. iOS background restrictions apply.
**Privacy:** Peer-to-peer through the OpenClaw gateway. No third-party servers store your location.

**Verdict:** The native answer. On-demand beats continuous streaming for most agent use cases. Worth investing in if you run OpenClaw daily.

## 4. Find My / iCloud Location APIs

**How:** Query Apple's Find My network via iCloud credentials or reverse-engineered APIs (e.g., `pyicloud`).

**Ease:** Fragile. Apple provides no official API. Two-factor auth complicates automation. Token expiry breaks unattended use.
**Reliability:** Poor. Apple changes endpoints without notice. Community libraries lag behind.
**Privacy:** You send your iCloud credentials to your own server — acceptable. But scraping an unofficial API invites breakage.

**Verdict:** Avoid. Too brittle for production use.

## 5. OwnTracks (Self-Hosted)

**How:** Run the OwnTracks iOS app. It publishes location via MQTT or HTTP to your own broker or server on the Pi.

**Ease:** Moderate setup. Install Mosquitto (MQTT broker), configure the app, set up TLS. One-time effort.
**Reliability:** Excellent. OwnTracks handles background location well on iOS — better than most apps. MQTT is lightweight and fast.
**Privacy:** Outstanding. Everything stays on your infrastructure. No cloud dependency.

**Verdict:** Best for continuous, private tracking. The gold standard if you want always-on location awareness.

---

## Recommendation

Start with **Telegram live location** — it works today with zero setup. For persistent, private tracking, deploy **OwnTracks**. For on-demand queries from the agent, invest in **OpenClaw node pairing**. Skip iCloud. Use **iOS Shortcuts** for geofence-triggered events that complement the others.

Match the tool to the need. No single method covers every case.
