---
title: Device tracker
description: Instructions on how to set up device tracking within Home Assistant.
ha_category:
  - Presence detection
ha_release: 0.7
ha_quality_scale: internal
ha_domain: device_tracker
ha_codeowners:
  - '@home-assistant/core'
ha_integration_type: entity
related:
  - docs: /integrations/person/
    title: Person
  - docs: /integrations/zone/
    title: Zone
---

The device tracker allows you to track devices in Home Assistant. This can happen by querying your wireless router or by having applications push location info.

{% include integrations/building_block_integration.md %}

To set up device tracking, add an integration that provides `device_tracker` entities, like the [Home Assistant Companion app](/integrations/mobile_app/) for phone-based location tracking or a router-based integration such as [Ubiquiti UniFi](/integrations/unifi/). You can connect device trackers to [person](/integrations/person/) entities and use them with [zones](/integrations/zone/) for automations that react when people or tracked devices enter or leave a place.

## The state of a tracked device

The type of state a device tracker can have depends on whether it uses GPS or a router as the data source.

A device tracker with **GPS** as a source can have any number of string states. The integration can return one of the following options:

- Report GPS coordinates. The coordinates are then matched to a zone (which is set as state). If the home zone is matched, the state will be **Home**. If no zone was matched the state will be **Not home**.
- Report a location. This could be any string which is set as state.

A device tracker with **router** as a source can have one of two states: **Home**, or **Not home**.

- **Home**: Your tracked device is in the [home zone](/integrations/zone/#about-the-home-zone), detected by your network or Bluetooth-based presence detection. If you're using a presence detection method that includes coordinates: when it's in a zone, the state equals the name of the zone (case sensitive).
- **Not home**: When a device isn't at home and isn't in any zone.

<p class='img'>
<img src='/images/integrations/device_tracker/state_device_tracker.png' alt='Screenshot showing the state of a device tracker entity in the developer tools' />
Screenshot showing the state of a device tracker entity in the developer tools.
</p>

In addition, the entity can have the following states:

- **Unavailable**: The entity is currently unavailable.
- **Unknown**: The state is not yet known.

## Automating tracked devices

You can use tracked devices in automations by connecting them to [person](/integrations/person/) entities and using [zone triggers](/integrations/zone/#triggers). This is the recommended path for presence automations because a person can combine multiple trackers, such as a phone and a router-based tracker, into one presence state.

Zone triggers can run an automation when a person or tracked device enters or leaves a zone. For example, you can turn on lights when you arrive home or send a notification when a tracked device leaves a school zone.

If you need to react to the raw state of one device tracker entity, use a [state trigger](/triggers/state/). Device tracker states depend on the integration that provides the entity. GPS-based trackers can report zones or custom location names, while router-based trackers usually report `home` or `not_home`.
