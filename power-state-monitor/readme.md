# Power State Monitor Blueprint

A flexible Home Assistant blueprint for monitoring power states of any appliance, with support for Google Sheets logging and smart notifications.

## ⚠️ Prerequisites

1. **Power Monitoring Device:**
   - Smart plug or power meter with:
     - Power readings in watts
     - Update frequency of 5 seconds or faster
     - ⚠️ **NOTE**: Some power meters only report current (A) or voltage (V). You can calculate power using a template sensor:

       ```yaml
       template:
         - sensor:
             name: "Device Power in Watts" 
             unit_of_measurement: "W"
             state: >
               {% set amps = states('sensor.device_amps') | float %}
               {% set volts = states('sensor.device_volts') | float %}
               {{ (amps * volts) | round(1) }}
       ```

     - ⚠️ **VERIFY**: Test power readings before proceeding with setup.

2. **Google Account:**
   - Google account with access to Google Drive
   - Google Cloud Console access
   - ⚠️ **IMPORTANT**: Keep OAuth credentials secure

## 🎯 Features

1. **Power State Detection:**
   - Working state (e.g., compressor, motor)
   - Extra state (e.g., door, water dispenser)
   - ⚠️ **NOTE**: Runs in parallel mode with configurable max instances
   - ⚠️ **IMPORTANT**: High update frequencies may impact performance

2. **Smart Notifications:**
   - Configurable timeout alerts
   - Mobile notifications
   - ⚠️ **VERIFY**: Test notifications after setup

3. **Google Sheets Logging:**
   - Automatic event logging
   - Duration tracking
   - ⚠️ **CRITICAL**: Do NOT use spaces or special characters in worksheet names
   - ⚠️ **NOTE**: Worksheet must exist before running automation

## 📊 Power Reference Values

1. **Two-State Devices** (Working/Idle):
   - Typical devices: Washing machines, dryers, freezers
   - Power pattern:
     - Idle: Very low power (0-5W)
     - Working: High power (200-1000W)
   - Examples:
     - Washing Machine: 1.2W idle, 350-500W washing
     - Dryer: 2-3W idle, 800-1000W drying
     - Freezer: 0-2W idle, 150-300W cooling
   - ⚠️ **NOTE**: Values may vary by model and manufacturer

2. **Three-State Devices** (Working/Idle/Extra):
   - Typical devices: Refrigerators with door lights, water dispensers
   - Power pattern:
     - Idle: Very low power (0-5W)
     - Working: Medium power (150-300W)
     - Extra activity: Small power spike (30-70W)
   - Example: Modern Fridge
     - Idle: 0-2W
     - Compressor ON: 150-250W
     - Door Open: +35W to +70W spike
     - Door Close: -35W to -70W drop
     - Water/Ice: +40W to +80W spike
   - ⚠️ **IMPORTANT**: Extra state detection requires measurable power change

## 🎯 How It Works

1. **Working State Detection:**
   - **ON Conditions:**
     - Power exceeds Working Power Threshold
     - Device not already marked as working
   - **OFF Conditions:**
     - Power is back to expected idle power
     - Device currently marked as working

2. **Extra State Detection:**
   - Can be disabled for two-state devices
   - **ON Detection:**
     - Power increase between Extra Power Min/Max
     - Extra state not already active
     - ⚠️ **NOTE**: Uses power change, not absolute value
   - **OFF Detection:**
     - Power decrease between -Extra Power Min/Max
     - Extra state currently active
     - 2-5 second delay for power stabilization (adjust based on your power sensor's update rate)

3. **Smart Features:**
   - State change validation to prevent duplicates
   - Power stabilization delays
   - Configurable timeout alerts
   - Optional cycle completion notifications

4. **Time Tracking:**
   - Start time recording for each state
   - Duration calculation in HH:MM:SS
   - Elapsed minutes for analysis

5. **Google Sheets Integration:**
   - **Logged Fields:**
     - Date (DD-Mon-YY)
     - Time (HH:MM:SS)
     - State Change
     - Power Change (watts)
     - Elapsed Time (HH:MM:SS)
     - Elapsed Minutes

## 🔧 Configuration

1. **Power Thresholds:**
   - Working state threshold
   - Extra state min/max
   - Idle power threshold
   - ⚠️ **IMPORTANT**: Monitor initial cycles to verify thresholds
   - ⚠️ **TIP**: Use the power graph of your device to understand typical power values and patterns
2. **Helper Entities:**
   - Create these helpers **before** using the blueprint
   - Go to **Settings > Devices & Services > Helpers > Create Helper**
   - Required helpers:
     - Two `input_boolean` entities for state tracking
     - Two `input_datetime` entities for timing
   - Example naming (for "kitchen_fridge"):
     - `input_boolean.kitchen_fridge_is_working`
     - `input_boolean.kitchen_fridge_extra_state`
     - `input_datetime.kitchen_fridge_working_started`
     - `input_datetime.kitchen_fridge_extra_started`
   - ⚠️ **VERIFY**: Create all required entities before activating

3. **Google Sheets Setup (Optional):**
   - Integration configuration
   - Worksheet setup
   - ⚠️ **SECURITY**: Store credentials in secrets.yaml
   - ⚠️ **NOTE**: New credentials may take time to activate

## 🔍 Troubleshooting

1. **Missing Events:**
   - Check power sensor update frequency
   - Verify threshold values
   - Monitor for overlapping states

2. **False Detections:**
   - Adjust power thresholds
   - Check for interference
   - Verify sensor accuracy
   - ⚠️ **NOTE**: Some trial and error may be needed

3. **Google Sheets Issues:**
   - Verify integration setup
   - Check worksheet name is simple (no spaces or special characters)
   - Validate credentials
   - ⚠️ **TIP**: Test with simple worksheet names
