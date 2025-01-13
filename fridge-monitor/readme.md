# Fridge Power Monitor

A Home Assistant implementation for monitoring fridge power consumption and logging activities to Google Sheets.

## ⚠️ Prerequisites

1. **Home Assistant Installation:**
   - Home Assistant with configuration directory access
   - ⚠️ **VERIFY**: Check Home Assistant version compatibility

2. **Power Monitoring Device:**
   - Smart plug or power meter with:
     - Power readings in watts
     - Update frequency of 5 seconds or faster
     - ⚠️ **CRITICAL**: Some smart plugs only report current (A) or voltage (V). These will NOT work.
     - ⚠️ **CRITICAL**: Ensure your power meter can handle the maximum current of your fridge.
     - ⚠️ **VERIFY**: Test power readings before proceeding with setup.

3. **Google Account:**
   - Google account with access to Google Drive
   - Google Cloud Console access
   - ⚠️ **IMPORTANT**: Keep OAuth credentials secure

## 📋 Implementation Files

1. **`fridge-power-monitor.yaml`**:
   - Main automation for monitoring power and logging state changes
   - Detects compressor and door events based on power thresholds
   - Logs events to Google Sheets with elapsed time
   - ⚠️ **NOTE**: Requires `secrets.yaml` configuration

2. **`fridge-door-alert.yaml`**:
   - Automation that alerts when the door is left open
   - Configurable timeout (default: 5 minutes)
   - Sends notifications to your phone
   - ⚠️ **NOTE**: Test notifications after setup

3. **`fridge-helper-entities.yaml`**:
   - Helper entities for state tracking
   - Input booleans for compressor and door states
   - Input datetimes for timing calculations
   - ⚠️ **IMPORTANT**: Create these before setting up automations

## 🔧 Setup Instructions

1. **Create Helper Entities:**
   - Copy the contents of `fridge-helper-entities.yaml` to your Home Assistant configuration
   - Restart Home Assistant to create the entities
   - ⚠️ **VERIFY**: Check that all helper entities are created successfully

2. **Set up Google Sheets Integration:**

   ### A. Configure Google Cloud Console

   1. Go to [Google Developers Console](https://console.developers.google.com/)
   2. Enable both **Google Drive API** and **Google Sheets API**
   3. Set up OAuth consent screen:
      - Select **External**
      - Set app name and contact info
      - Set status to **In production**
   4. Create OAuth credentials:
      - Create **Web application** credentials
      - Add redirect URI: `https://my.home-assistant.io/redirect/oauth`
      - Save your **Client ID** and **Client secret**

   ### B. Configure Home Assistant

   - Go to **Settings > Devices & Services > Add Integration > Google Sheets**
   - Enter your OAuth credentials and follow the authentication process
   - Note: You need access to Google Drive and Google Cloud Console
   - Important: Use simple worksheet names without spaces or special characters
   - The integration will automatically create required headers

   ⚠️ **IMPORTANT**: New credentials may take up to 5 hours to activate

3. **Configure Secrets:**
   - Create or edit `secrets.yaml` in your Home Assistant config directory
   - To get your `config_entry_id`:
     1. Create a new automation in Home Assistant
     2. Add an action
     3. Search for "Google Sheets append to sheet"
     4. Select your sheet from the dropdown menu
     5. Switch to YAML mode
     6. Copy the `config_entry` value shown in the action
   - Add to your `secrets.yaml`:

     ```yaml
     # Google Sheets Configuration
     google_sheets_config_entry_id: "YOUR_CONFIG_ENTRY_ID"  # Get this from the automation YAML
     ```

   - ⚠️ **SECURITY**: Never share your `secrets.yaml` file
   - ⚠️ **IMPORTANT**: Add `secrets.yaml` to your `.gitignore`

4. **Configure Worksheet:**
   - The default worksheet name is "Sheet1"
   - You can change this by updating the `worksheet_name` variable in `fridge-power-monitor.yaml`
   - Example names: "Sheet1", "W1", "FridgeLog"
   - ⚠️ **CRITICAL**: Do NOT use spaces or special characters in worksheet names
   - ⚠️ **VERIFY**: The worksheet must exist before running the automation

5. **Configure Power Sensor:**
   - Update `sensor.fridge_power` in the automations to match your power sensor
   - Sensor must provide power readings in watts
   - Should update frequently (at least every 5 seconds)
   - Recommended: Use a power meter with 1-second updates
   - ⚠️ **VERIFY**: Check that your sensor reports values correctly
   - ⚠️ **IMPORTANT**: Validate sensor state values are not 'unknown' or 'unavailable'

6. **Configure Notifications:**
   - Update the notification service in `fridge-door-alert.yaml`
   - Default: `notify.mobile_app` (change to your device's notify service)

## 🎯 Default Power Thresholds

- **Compressor Detection:**
  - ON: Power > 200W
  - OFF: Power < 20W
  - ⚠️ **NOTE**: These thresholds may need adjustment for your specific fridge model

- **Door Events:**
  - Open: Power increase between 30W and 70W
  - Close: Power decrease between -30W and -70W
  - ⚠️ **IMPORTANT**: These values assume a door light is present and it is not LED.

## 📊 Power Reference Values

Modern Fridge Power States:

- Idle (Compressor OFF): 0-2W
- Working (Compressor ON): 150-250W
- Door Open: +35W to +70W spike
- Door Close: -35W to -70W drop
- Water/Ice Dispenser: +40W to +80W spike
- ⚠️ **NOTE**: Your values may vary based on fridge model and features

## 🔍 State Detection Logic

1. **Compressor State:**
   - ON when power exceeds 200W and compressor was off
   - OFF when power drops below 20W and compressor was on
   - Logs duration when turning off
   - ⚠️ **NOTE**: Brief power fluctuations are filtered out
   - ⚠️ **IMPORTANT**: Monitor initial cycles to verify thresholds

2. **Door State:**
   - OPEN when power increases by 30-70W and door was closed
   - CLOSED when power decreases by 30-70W and door was open
   - ⚠️ **CRITICAL**: 2-second delay on door close for power stabilization
   - ⚠️ **NOTE**: Door events require a working door light
   - Logs duration when closing

## 📝 Notes

- Implementation is optimized for refrigerators with door lights
- Power thresholds may need adjustment for your specific model
- Door events are detected by the power change from the door light
- Google Sheets logging includes timestamps and durations
- Alert system notifies if door left open over 5 minutes

## ⚠️ Troubleshooting

1. **Missing Events:**
   - Verify power sensor update frequency
   - Check if power thresholds match your fridge
   - Look for overlapping events in logs

2. **False Detections:**
   - Increase minimum power thresholds
   - Check for other appliances on same circuit
   - Verify door light functionality

3. **Google Sheets Issues:**
   - Confirm worksheet exists and is accessible
   - Check `config_entry_id` in secrets
   - Verify Google Sheets integration status
   - Check OAuth consent screen is "In production"
   - Verify both Drive and Sheets APIs are enabled
   - Check redirect URI is exactly correct
   - Try deleting and recreating credentials if issues persist

4. **Notification Problems:**
   - Check mobile app configuration
   - Verify notification service name
   - Test with Home Assistant companion app

5. **Power Reading Issues:**
   - Verify sensor provides watts (not amps or volts)
   - Check sensor update frequency
   - Monitor for 'unknown' or 'unavailable' states
   - ⚠️ **TIP**: Use Developer Tools to watch power values
   - ⚠️ **NOTE**: If sensor provides amps and volts separately, create a template sensor:

     ```yaml
     template:
       - sensor:
           name: "Fridge Power in Watts"
           unit_of_measurement: "W"
           state: >
             {% set amps = states('sensor.fridge_amps') | float %}
             {% set volts = states('sensor.fridge_volts') | float %}
             {{ (amps * volts) | round(1) }}
     ```

6. **Event Detection Issues:**
   - Check power thresholds match your fridge
   - Monitor power graphs to determine optimal delay timing
   - Adjust delay if events are being missed (5-10 seconds may be needed)
   - Verify parallel mode is working correctly
   - ⚠️ **TIP**: Use Energy dashboard or Developer Tools to analyze power patterns

For a more generic implementation that can be used with other appliances, see the [Power State Monitor Blueprint](../power-state-monitor/power-state-monitor-blueprint.yaml).
