# Feature Analysis: Safety Hack (Women's Safety Mode)

This report evaluates the proposal for a "Safety Hack" feature designed to protect women in low-battery or high-risk situations.

## Executive Summary
The "Safety Hack" is a high-impact safety feature that activates when a user is vulnerable (e.g., outside with low battery). While most aspects are technically feasible, system-level restrictions on mobile OSs (iOS/Android) prevent standard apps from blocking other apps. We propose an **"Enhanced Safety Mode"** that achieves the user's goals through legal and technically viable methods.

---

## Technical Feasibility Breakdown

### 1. Low Battery Trigger (可行 - FEASIBLE)
- **Technology**: `expo-battery`
- **Capability**: The app can monitor battery levels in real-time. When it drops below a threshold (e.g., 10% or 15%), the app can trigger a notification or a "Safety Mode" prompt.
- **Limitation**: Background monitoring depends on the OS's power management; it is most reliable when the app is open or recently used.

### 2. Live Location Sharing (可行 - FEASIBLE)
- **Technology**: `expo-location` + Backend (FastAPI/MongoDB)
- **Capability**: We can capture the user's high-accuracy GPS coordinates and send them to the backend every 30-60 seconds.
- **Limitation**: Requires "Always" location permissions from the user. iOS/Android are strict about this, but for a safety app, users typically grant it.

### 3. Emergency Contact Notification (可行 - FEASIBLE)
- **Technology**: Backend SMS Integration (e.g., Twilio) or Push Notifications.
- **Mechanism**: The backend can automatically send an SMS to saved contacts with a Google Maps link to the live location.

### 4. Blocking Other Apps (限制 - RESTRICTED)
- **The Challenge**: For security and privacy reasons, mobile operating systems (especially iOS) **do not allow** one app to prevent other apps from opening. This is to prevent "malware" from taking over a phone.
- **Android Exception**: Only "Launcher" apps or "Device Admin" apps (used by companies) can do this. A standard app cannot.
- **Proposed "Safety Override"**: Instead of blocking apps, we create a **Full-Screen Distressed UI**. This UI covers the screen, plays a loud alarm (if needed), and provides a "Fake Lock" appearance to deter attackers or simplify the phone to only emergency functions.

---

## Proposed Enhanced "Safety Mode" Design

Rather than a simple "Yes/No" for accidental touch, we propose a more robust workflow:

1.  **Detection**: App detects battery < 15% AND user is away from "Home" location.
2.  **The Prompt**: A prominent, high-contrast overlay asks: *"Low Battery Detected. Activate Safety Mode?"*
3.  **Activation (User clicks "Yes")**:
    *   **SOS Broadcast**: Immediate SMS sent to 3 saved emergency contacts with live location.
    *   **Power Saver UI**: App switches to a black-background UI (to save battery) with only 3 buttons: [Call Police], [Call Contact], and [Share Live Location].
    *   **App Lock Simulation**: The app stays in the foreground. If the user tries to leave, a "Safety Mode Active" sticky notification remains.
4.  **Cancellation (User clicks "No")**: App continues as normal.

---

## Required Project Additions

To implement this, we need:
1.  **Emergency Contact Screen**: A new section in the Profile to save name/number of 3 trusted contacts.
2.  **Background Task**: Implementation of `expo-task-manager` to handle location updates when the app is backgrounded.
3.  **SMS Gateway**: Integration with a service like Twilio on the Python backend.

---

## Analysis Conclusion & Next Steps
The feature is **Highly Recommended** as it adds unique value to the `LearnRights` project beyond education. 

**Next Steps for Implementation:**
1.  Verify the user's preference for the "Accidental Touch" wording.
2.  Integrate `expo-battery` into `HomeScreen.js`.
3.  Design the "Safety Hub" UI in the mobile app.
4.  Setup the backend "Emergency Notification" service.

---

## 🚀 Advanced Creative Enhancements (Phase 2)

To make this feature truly "Effective" and stand out, we can consider these advanced additions:

### 1. Voice-Triggered SOS (Keyword Activation)
- **Idea**: Allow the user to record a "Safe Word" (e.g., "Eagle SOS"). If the app is open and hears this, it triggers the broadcast without requiring a touch.
- **Benefit**: Essential if the user's hands are restrained or they cannot reach the phone.

### 2. "Fake Call" Generator
- **Idea**: A button in the Safety Hub that triggers a realistic-looking incoming call screen after 10 seconds.
- **Benefit**: Provides a socially acceptable "excuse" to walk away from an uncomfortable conversation or situation.

### 3. Stealth Mode (Hidden UI)
- **Idea**: A setting to disguise the Safety Hub as a harmless "Calculator" or "Weather App" within the phone's app drawer.
- **Benefit**: Prevents an attacker from identifying and deleting the safety app.

### 4. Background Audio Evidence
- **Idea**: When SOS is triggered, the app automatically records 30 seconds of audio and uploads it to the backend.
- **Benefit**: Provides crucial evidence of what was happening during the distress period.

### 5. "Safe Haven" Map Integration
- **Idea**: In Safety Mode, the map highlights the nearest 24/7 pharmacies, hospitals, and police stations.
- **Benefit**: Gives the user a clear destination to find help and security.

### 6. Battery-Optimized "Ghost" Mode
- **Idea**: If the battery is extremely low (below 5%), the app sends a "Final Location" toast and then switches to a text-only mode to preserve the last few minutes of life for emergency calls.
