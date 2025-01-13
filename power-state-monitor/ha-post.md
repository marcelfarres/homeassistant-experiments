# Power State Monitor Blueprint - Smart Appliance Monitoring with Google Sheets Integration

## 🎯 Overview

Monitor any appliance's power states, log activities to Google Sheets, and receive smart notifications. Perfect for fridges, freezers, washing machines, dryers, and more! Turn any power-monitored device into a smart device with state detection and logging.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](upload://3IEiMyDuriGlhMmaFV0iSnXlL0b.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://gist.github.com/marcelfarres/41aa967977aae9f593caf28f7618c639)

## ✨ Key Features

1. **Smart Power State Detection**
   - Automatic detection of working/idle states
   - Optional extra state detection (e.g., door events)
   - Precise power change monitoring
   - Configurable thresholds for any device
   - Power stabilization delays for accuracy
   - Parallel execution with configurable instances

2. **🔔 Intelligent Notifications**
   - Extra state timeout warnings (e.g., door left open)
   - Cycle completion alerts (e.g., washer finished)
   - Mobile app & web UI notifications
   - Persistent notifications until acknowledged
   - Customizable notification titles

3. **📊 Google Sheets Integration**
   - Automatic logging of all state changes
   - Power change tracking
   - Elapsed time calculations in minutes & (HH:MM:SS)
   - Customizable worksheet names

4. **⚡ Flexible Configuration**
   - Adjustable power thresholds
   - Custom state names

## 💡 Real-World Applications

### 1. Kitchen Appliances

- **Fridge/Freezer Monitoring**
  - Track compressor cycles
  - Detect door open/close events
  - Get alerts for door left open
  - Analyze cooling patterns

### 2. Laundry Management

- **Washer/Dryer Monitoring**
  - Know exactly when cycles finish & get alerts
  - Track cycle durations

### 3. Energy Analysis

- **Power Usage Insights**
  - Track consumption patterns
  - Monitor standby power
  - Historical data analysis

## 📊 Power Reference Values

### Two-State Devices

- **Washing Machine**: 1.2W idle, 350-500W working
- **Dryer**: 2-3W idle, 800-1000W working
- **Freezer**: 0-2W idle, 150-300W working

### Three-State Devices

- **Modern Fridge**:
  - Idle: 0-2W
  - Compressor: 150-250W
  - Door Events: 35-70W change
  - Water/Ice: 40-80W spike

## 🔧 Requirements

1. **Home Assistant**
   - Mobile app (optional, for notifications)

2. **Hardware**
   - Power monitoring device (watts)
   - 1-5 second update rate recommended
   - Compatible with Home Assistant
   - ⚠️ For volt/amp meters, use template sensor

3. **Google Account**
   - Google Drive access
   - Google Cloud Console access
   - OAuth 2.0 credentials

## 📝 Setup Instructions

Detailed setup instructions are included in the blueprint, covering:

1. **Google Sheets Integration**
   - OAuth configuration
   - Worksheet setup
   - Integration configuration
   - Getting the config_entry_id (Easiest Method):
     1. Create a new automation in Home Assistant
     2. Add an action
     3. Search for "Google Sheets append to sheet"
     4. Select your sheet from the dropdown menu
     5. Switch to YAML mode
     6. The `config_entry` value will be shown in the action
     7. Copy this ID for use in your automations
   - ⚠️ **Note**: Keep this ID in your `secrets.yaml` file

2. **Helper Entities**
   - State tracking booleans
   - Timing entities
   - Naming conventions
   - Entity configuration

3. **Power Configuration**
   - Threshold settings
   - State detection tuning
   - Update frequency
   - Stabilization delays

4. **Notification Setup**
   - Mobile devices selection
   - Web UI options
   - Alert timeouts
   - Custom messages

## 🤝 Contributing

Feel free to:

- Share your experience
- Suggest improvements
- Report issues
- Contribute to development

## 📄 Links

- [GitHub Gist](https://gist.github.com/marcelfarres/41aa967977aae9f593caf28f7618c639)

## 🙏 Acknowledgments

Special thanks to the Home Assistant community for inspiration.
