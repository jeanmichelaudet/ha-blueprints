# Home Assistant Blueprints

Personal collection of Home Assistant blueprints for automation, scripts, and templates.

## Blueprints

### Automation

| Blueprint | Description | Import |
|-----------|-------------|--------|
| [Aqara Switch Matter](blueprints/automation/jeanmichelaudet/aqara-switch-matter.yaml) | Handle Aqara switch button events via Matter protocol | [![Import](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/jeanmichelaudet/ha-blueprints/main/blueprints/automation/jeanmichelaudet/aqara-switch-matter.yaml) |

### Scripts

| Blueprint | Description | Import |
|-----------|-------------|--------|
| *Coming soon* | | |

## Prerequisites

- **Home Assistant** 2022.5.0 or newer (required for `not_from`/`not_to` trigger options)
- **Matter integration** configured and working
- **Aqara switch** paired via Matter (creates `event.*` entities with button device class)

## Installation

### Option 1: One-click import (recommended)

Click the "Import Blueprint" button in the table above.

### Option 2: Manual import

1. Go to **Settings** > **Automations & Scenes** > **Blueprints**
2. Click **Import Blueprint**
3. Paste the raw URL of the blueprint:
   ```
   https://raw.githubusercontent.com/jeanmichelaudet/ha-blueprints/main/blueprints/automation/jeanmichelaudet/aqara-switch-matter.yaml
   ```

### Option 3: File copy

Copy the YAML file to your Home Assistant config directory:
```
config/blueprints/automation/jeanmichelaudet/aqara-switch-matter.yaml
```

## Example

After importing the Aqara Switch Matter blueprint, create an automation like this:

```yaml
alias: "Kitchen Aqara Switch"
description: "Control kitchen lights with Aqara switch"
use_blueprint:
  path: jeanmichelaudet/aqara-switch-matter
  input:
    switch_entity: event.aqara_switch_kitchen_button
    single_press_action:
      - service: light.toggle
        target:
          entity_id: light.kitchen_main
    double_press_action:
      - service: scene.turn_on
        target:
          entity_id: scene.kitchen_bright
    long_press_action:
      - service: light.turn_off
        target:
          area_id: kitchen
    long_release_action: []
    automation_mode: single
```

## Troubleshooting

### Blueprint doesn't trigger

1. **Verify entity type**: The entity must be an `event` domain entity (e.g., `event.aqara_switch_button`), not a `sensor` entity
2. **Check entity attributes**: Go to **Developer Tools** > **States**, find your entity, and verify it has an `event_type` attribute
3. **Confirm Matter hub status**: The blueprint filters out `unavailable`/`unknown` states. Ensure your Matter hub is online
4. **Test the button**: Press the button and watch the entity state change in Developer Tools

### Wrong action fires

1. **Check event_type values**: In Developer Tools > States, watch the `event_type` attribute when pressing the button
2. **Aqara event mapping**:
   - Single press: `multi_press_1`
   - Double press: `multi_press_2`
   - Long press (hold): `long_press`
   - Long release: `long_release`

### Actions run multiple times

1. **Check automation mode**: Set to "Single" to ignore new triggers while running
2. **Verify trigger conditions**: Ensure no other automations are controlling the same devices

### Entity not showing in selector

1. The selector filters for `event` domain with `button` device class
2. Ensure your Aqara switch is properly paired via Matter
3. Check if the entity exists under **Settings** > **Devices & Services** > **Matter**

## Contributing

These are personal blueprints, but feel free to fork or suggest improvements via issues.

## License

MIT License - See [LICENSE](LICENSE) for details.
