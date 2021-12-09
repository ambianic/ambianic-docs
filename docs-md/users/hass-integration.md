
# Home Assistant Integration

[Home Assistant](https://www.home-assistant.io/) is one of the most popular Open Source Home Automation hubs and it is a natural integration target for Ambianic. The integration enables users to implement new home automation flows based on Ambianic detection events. For example:
* Turn on lights if a family member walks in the house and its night time.
* Trigger the alarm if an unknown person is on private property for more than 1 minute.
* Turn off the stove if no person has been detected in the house for over an hour; or
* Instantly notify family and caregivers if an elderly loved one falls.
* Open the garage door when a family owned car pulls up in the driveway.

## Notifications

The simplest way to integrate Ambianic with Home Assistant is via notifications. 

If you prefer to configure notifications via the Ambianic UI, then you can setup an IFTTT applet to forward Ambianic Edge notifications to your local Home Assistant instance or via the Nabu Casa cloud API. Follow the [Ambianic UI Notifications setup guide](https://docs.ambianic.ai/users/notifications/) to send notifications from Ambianic Edge to IFTTT and then follow the [Home Assistant's IFTTT integration guide](https://www.home-assistant.io/integrations/ifttt/) to forward notifications from IFTTT to Home Assistant.

You can also configure direct local integration between an Ambianic Edge device and a local Home Assistant instance. This is a bit more advanced and requires access to the Ambianic Edge on-device configuration file. See the [advanced notifications configuration guide](https://docs.ambianic.ai/users/configure/#notification-settings) for details.

## Add-on

Ambianic can also be installed as a Home Assistant Add-on. See details [here](https://github.com/dcmartin/addon-ambianic).
