# Home Assistant Experiments

A collection of Home Assistant automations, blueprints, and configurations for various home automation projects.

## 📑 Projects

### 1. Power State Monitor

Monitor power consumption of appliances and log their activities to Google Sheets. Available in two versions:

#### A. Generic Blueprint (`/power-state-monitor`)

A flexible blueprint that can monitor any appliance with:

- Working state (e.g., compressor, washing, drying)
- Extra state (e.g., door, water, heating)
- Configurable state names and thresholds
- Smart notifications and Google Sheets logging

Perfect for users who want a plug-and-play solution that can be reused for different appliances.

#### B. Fridge Implementation (`/fridge-monitor`)

A specific implementation optimized for refrigerators that tracks:

- Compressor cycles (ON/OFF)
- Door events (Open/Close)
- Includes door-open alerts
- Preconfigured power thresholds

Good example of how to implement the generic blueprint for a specific appliance.

For detailed documentation, see:

- [Generic Blueprint Documentation](power-state-monitor/readme.md)
- [Fridge Implementation Documentation](fridge-monitor/readme.md)
