# Power State Monitor

## 📋 Implementation Options

This project offers two ways to implement the power state monitoring:

1. **Blueprint Implementation** (`fridge-monitor-blueprint.yaml`):
   - Single file containing all logic and configuration
   - Easy to import through Home Assistant UI
   - Configurable through a user-friendly interface
   - Includes smart notifications out of the box
   - Recommended for most users

2. **Traditional Implementation** (`logging-automation.yaml` + `configuration.yaml`):
   - Split into separate automation and configuration files
   - More direct and transparent implementation
   - Easier to modify and customize
   - Good for learning how the automation works
   - Recommended for advanced users or learning purposes

The documentation below focuses on the Blueprint implementation, which is the recommended approach for most users.

## 🎯 Overview

This blueprint monitors power consumption of appliances and logs their activities to Google Sheets. It's designed to work with both two-state devices (like freezers) and three-state devices (like refrigerators with door lights).

### **Key Features:**

1. **Power State Detection:**
   - Detects working/idle states (compressor)
   - Door open/close detection
   - Logs all events to Google Sheets
   - Smart notifications

2. **Notifications:**
   - Door left open warnings (configurable timeout)
   - Cycle completion alerts (e.g., washer finished)
   - Mobile app & web UI notifications
   - Persistent notifications until acknowledged

### **Key Points:**

1. **Triggers:**
   - The automation is triggered **whenever the power sensor changes its state**.

2. **State Detection:**
   - **Compressor ON:**
     - **Conditions:**
       - Power > Working Power Threshold (default: 200W)
       - Compressor is not already marked as ON
     - **Actions:**
       - Turn on the compressor `input_boolean`
       - Set the compressor start time
       - Log "Compressor ON" to Google Sheets

   - **Compressor OFF:**
     - **Conditions:**
       - Power < Idle Power Threshold (default: 5W)
       - Compressor is currently marked as ON
     - **Actions:**
       - Turn off the compressor `input_boolean`
       - Log "Compressor OFF" with elapsed times
       - Send notification if cycle completion alerts enabled
       - Clear the compressor start time

   - **Door Open:**
     - **Conditions:**
       - Power increase between Extra Power Min and Max (default: 30-70W)
       - Door is not already marked as open
     - **Actions:**
       - Turn on the door `input_boolean`
       - Set the door opened time
       - Log "Door Open" to Google Sheets
       - Start door timeout monitoring if enabled

   - **Door Closed:**
     - **Conditions:**
       - Power decrease between -Extra Power Min and -Extra Power Max
       - Door is currently marked as open
     - **Actions:**
       - 2-second delay to allow power stabilization
       - Turn off the door `input_boolean`
       - Log "Door Closed" with elapsed times
       - Clear any door timeout notifications

3. **Smart Features:**
   - **Preventing Duplicate Entries:**
     - Uses `input_boolean` entities to track current states
     - Ensures state changes are logged only once

   - **Door Event Detection:**
     - Immediate detection of door open events
     - 2-second delay for door close events
     - Handles multiple power changes during delay

   - **Notifications:**
     - Configurable door timeout (default: 5 minutes)
     - Optional cycle completion alerts
     - Both mobile and web UI notifications
     - Automatic notification clearing

4. **Time Tracking:**
   - **Elapsed Time Calculation:**
     - Human-readable format (HH:MM:SS)
     - Machine-readable format (minutes)
     - Automatic error handling for missing timestamps

5. **Google Sheets Integration:**
   - **Worksheet Structure:**
     - **Date:** DD-Mon-YY format
     - **Time:** HH:MM:SS format
     - **State:** Current state change
     - **Power Change:** Power difference in watts
     - **Elapsed Time:** Duration in HH:MM:SS
     - **Elapsed Minutes:** Duration in minutes

6. **Power Reference Values:**
   - **Two-State Devices:**
     - Washing Machine: 1.2W idle, 350-500W working
     - Dryer: 2-3W idle, 800-1000W working
     - Freezer: 0-2W idle, 150-300W working

   - **Three-State Devices:**
     - Modern Fridge:
       - Idle: 0-2W
       - Compressor: 150-250W
       - Door Events: 35-70W change
       - Water/Ice: 40-80W spike

---

For detailed setup instructions and configuration options, please refer to the blueprint description in Home Assistant.
