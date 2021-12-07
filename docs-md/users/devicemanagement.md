# Managing Ambianic Edge devices

One or more Ambianic Edge devices can be managed via the Ambianic UI app. Each Ambianic Edge device operates completely autonomously. 
The Ambianic UI provides a convenient way to configure individual Ambianic Edge devices, inspect their timelines and provide user feedback to AI tasks for on-device training.

# Adding a New Device

To add a new device in the UI, go to _Settings > My Devices > Add Device_.

![Screen Shot 2021-12-07 at 9 44 50 AM](https://user-images.githubusercontent.com/2234901/145060941-20c2fe05-de18-4cdf-9f0e-154f59fe1dc3.png)

![Screen Shot 2021-12-07 at 9 44 57 AM](https://user-images.githubusercontent.com/2234901/145060958-4e2b5e75-fefa-41ab-a2fe-54fa1a7eca34.png)

![Screen Shot 2021-12-07 at 9 45 08 AM](https://user-images.githubusercontent.com/2234901/145060979-e3ac0e60-2377-4c5b-8c97-4d42462121eb.png)

You can add a new Ambianic Edge device in the UI app either via local WiFi network discovery or directly via device Peer ID.

## Local Discovery

In order for Ambianic UI to discover a local Ambianic Edge device, the UI browser and edge device must be connected on the same local network sharing
the same Internet router and public IP address. That enables them to join a shared discovery room in the Ambianic Plug-and-Play web service. See diagram below.

![Ambianic Edge Local Discovery Diagram drawio](https://user-images.githubusercontent.com/2234901/145060316-f1f1ab63-6a11-43a8-b079-29efe54c6555.png)

If this is the case, choose _Local_ and pick from the list of discovered local devices. They will be listed by their unique Peer ID number for privacy protection. 

![Screen Shot 2021-12-07 at 9 45 24 AM](https://user-images.githubusercontent.com/2234901/145061525-3a5eae5e-aafa-4207-a18e-7777f2fb50e6.png)

![Screen Shot 2021-12-07 at 9 45 39 AM](https://user-images.githubusercontent.com/2234901/145061540-2d4a59a8-3cf7-4222-90f8-0f54ae1a6378.png)

# Direct Peer ID Connection

If you are connecting to a device that is not on the same local network as the browser runnning Ambnianic UI, you will need to obtain the Peer ID of the remote device.
Pick _Remote_ in the Add Device wizard and enter the remote Peer ID.

![Screen Shot 2021-12-07 at 9 54 47 AM](https://user-images.githubusercontent.com/2234901/145062269-0051d18b-562d-44ea-8b93-45016bc25603.png)

# Configure a New Device

Once an initial device connection is established, you can click on _Settings_ to proceed to the device configuration page or pick _Timeline_ to inspect the event timeline recorded on the device.

The device configuration page allows you to customize the device Friendly Name, setup [Notifications](https://docs.ambianic.ai/users/notifications/) and other preferences.

![Screen Shot 2021-12-07 at 9 45 46 AM](https://user-images.githubusercontent.com/2234901/145061570-a262c30d-0a6c-4044-954b-4ad7ae3e2bd8.png)

# Switching Devices

Ambianic UI allows you to choose and interact with one Ambianic Edge device at a time. You can switch the current device by going to _Settings > My Devices > Switch_

![Screen Shot 2021-12-07 at 10 02 45 AM](https://user-images.githubusercontent.com/2234901/145063626-cc2ce458-6d77-4490-8e6a-c711c6bceca9.png)

The friendly device name of the currently selected device is shown in the app bar for your convenience.

