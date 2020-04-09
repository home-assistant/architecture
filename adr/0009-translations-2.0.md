# 0009. Translations 2.0

Date: 2020-04-08

## Status

Accepted

## Context

The backend has only one API endpoint to fetch translations: `frontend/get_translations`. It takes no parameters. This needs to load all relevant translations. Because of config flows, relevant translations are all translations of all our currently loaded integrations PLUS any integration that has a config flow.

With so many integrations with config flows, this API call is getting huge. On current dev, the response is 142KB! ([truncated example](https://hastebin.com/raw/aturojejez))

Our main translation files (`strings.json`) is keyed by "area" where they are used. There is:
- `config` for config flow
- `options` for option flow
- `device_automation` for device automations
- `state` for translations of states under your domain (example is entity `zwave.bla` with state `alive`). This is not used by any `strings.json`.

Integrations can also have platform specific translation files (`strings.sensor.json`). These files are merged into the integration that they are offering a platform to. So the values of `strings.sensor.json` will be merged into `sensor/strings.json`. This is only used to provide `state`. This is only useful for sensors with text values and is only used by [moon](https://github.com/home-assistant/core/blob/dev/homeassistant/components/moon/strings.sensor.json) and [season](https://github.com/home-assistant/core/blob/dev/homeassistant/components/season/strings.sensor.json). 

The title of integrations sometimes requires being translated too (mainly internal integrations) and so is part of our translation file. However we made the mistake of putting the `title` under `config`. So even if we would open up our API to allow fetching just one area, we would still need to fetch all config flow info of all integrations in case we need to render the name of a single domain 🤷‍♂.

We should drop the `title` from `strings.json` if the integration is for a product who'se name does not require translation and so it should remain equal to the title in `manifest.json`. Examples are `zwave` or `hue`. 

## Decision

#### Backend

- Move integration title out of `config` key so we can load it independently ([current PR](https://github.com/home-assistant/core/pull/33850))
- Drop all titles from `strings.json` that are the same as `manifest.json` and should not be translated.
- Remove support for integrations providing platform translations
- When fetching titles for integrations, use the manifest name if title is not provided in translation file.
- Change `frontend/get_translations` to allow specifying area that you are interested in
- We need to start using references and write specific translations down once. Right now each integration defines `already_configured` etc. We use this syntax in the frontend: `[%key:state::binary_sensor::battery::off%]`.
- Update scaffold scripts to use references.
- Start using `state` in `strings.json`. It is going to be a dictionary that has a `default` entry and one entry per device class. Just like we have now in the frontend.
    ```json
    {
      "state": {
        "binary_sensor": {
          "default": {
            "off": "[%key:state::default::off%]",
            "on": "[%key:state::default::on%]"
          },
          "battery": {
            "off": "Normal",
            "on": "Low"
          },
        }
      }
    }
    ```
- Import all existing state keys + translations from frontend
- Add a new `state_attributes` to `strings.json` to allow offering translations for state attributes.
  ```json
  {
    "state_attributes": {
      "climate": {
        "hvac_action": {
          "off": "Off",
          "heating": "Heating",
          "cooling": "Cooling",
          "drying": "Drying",
          "idle": "Idle",
          "fan": "Fan"
        }
      }
    }
  }
  ```
- Import all existing state attribute keys + translations from frontend

#### Frontend
Load part of backend translations when necessary.

- On page load, load `state` only.
- Fetch integration titles when opening config or dev tools panels
- Fetch config flow translations when config flow started (not handler picked)
- Fetch options flow translations when option flow started
- Fetch device automation strings when opening script/automation editor
- Remove built-in translations of domains (`domain.*`) and use backend provided strings instead.
- Remove built-in translations for states (`state`) and use backend provided strings instead.
- Remove built-in translations for state attributes (`state_attributes`) and use backend provided strings instead.
- Adopt cleaning keys script from backend ([see this PR](https://github.com/home-assistant/core/pull/33802))
- Remove all state and state attributes keys from translations once imported into backend

## Consequences

- Frontend will load faster and use less memory
- Couple translations of integrations to the integration instead of the frontend release

The `moon` and `season` integrations will no longer have their entities translated. I suggest that if we see a need to bring this back, we could consider migrating `moon` and `season` to not write sensor entities but instead have them write states under their own domain (`moon.moon`) like the `sun` integration does. They can then add `state` to their own `strings.json` which can then be loaded when the frontend loads.
