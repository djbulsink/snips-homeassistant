# snips-homeassistant

Snips integration with Home Assistant on Raspberry Pi.

For a detailed tutorial, check out our blog post: [Integrating Snips with Home Assistant](https://medium.com/p/314723645c77).

# Setup

## Home Assistant

Check out the [Getting Started Guide](https://home-assistant.io/getting-started/) for setting up Home Assistant on a Raspberry Pi.

Once set up, copy `snips.py` to your local `custom_components` folder:

```sh
$ scp snips.py pi@pi_hostname:/home/homeassistant/.homeassistant/custom_components
```

By default, the Snips MQTT broker runs on port 9898. We should tell Home Assistant to use this as a broker (rather than its own), by adding the following section to `configuration.yaml`:

```yaml
mqtt:
  broker: 127.0.0.1
  port: 9898
```

## Snips Voice Platform

### Installation

The Snips Voice Platform is installed with the following command:

```sh
$ curl https://install.snips.ai -sSf | sh
```

### Creating an assistant

Snips assistants are created via the [Snips Console](console.snips.ai). Once trained, the assistant should be downloaded and copied to the Raspberry Pi

```sh
$ scp assistantproj_XXX.zip pi@pi_hostname:/home/pi/assistant.zip
```

and installed locally on the Raspberry Pi via the `snips-install-assistant` helper script:

```sh
$ ssh pi@pi_hostname
$ sudo snips-install-assistant assistant.zip
```

### Running Snips

Make sure that a microphone is plugged to the Raspberry Pi, and start the Snips Voice Platform using the `snips` command:

```sh
$ snips
```

## Home Assistant actions

In Home Assistant, we trigger actions based on intents produced by Snips. This is done in `configuration.yaml`. For instance, the following block handles `ActivateLightColors` intents (included in the Snips IoT intent bundle) to change light colors in the house:

```yaml
snips:
  intents:
    ActivateLightColor:
      action:
        - service: light.turn_on
          data_template:
            entity_id: light.{{ objectLocation | replace(" ","_") }}
            color_name: {{ objectColor }}
```

# Support

Please file issues on the [Issue Tracker](https://github.com/snipsco/snips-homeassistant/issues), or get in touch directly on our [Snips Slack Channel](https://snipslabs.slack.com/).