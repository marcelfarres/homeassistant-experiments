# Home Assistant Experiments

A collection of Home Assistant automations, blueprints, and configurations for various home automation projects.

## 📑 Projects

### 1. Power State Monitor (`/fridge-monitor`)

Monitor power consumption of appliances and log their activities to Google Sheets. Perfect for fridges, freezers, washing machines, and more.

**Two Implementation Options:**

1. **Blueprint Version** (`fridge-monitor-blueprint.yaml`):
   - Single-file solution using Home Assistant's blueprint system
   - Easy to import and reuse across different devices
   - Configurable through the UI
   - Includes smart notifications and Google Sheets logging
   - Perfect for users who want a plug-and-play solution

2. **Traditional Version** (`logging-automation.yaml` + `configuration.yaml`):
   - Classic Home Assistant automation approach
   - Split into separate configuration and automation files
   - More direct control over the implementation
   - Good for learning and customization
   - Better for users who want to understand the underlying mechanics

For detailed documentation, see the project's [README](fridge-monitor/readme.md).
