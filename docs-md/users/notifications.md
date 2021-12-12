
# Ambianic Notifications

Each Ambianic Edge device can be configured to independently send instant notifications.

The simplest way to configure an Ambianic Edge device to send FREE notifications is via the Ambianic UI PWA and IFTTT. Follow the instructions below to setup and test your first notification message via IFTTT integration.

## Get an IFTTT Webhook API KEY

[IFTTT](https://ifttt.com) is a popular notification hub that provides a convenient way to create automation workflows for SMS, Email, 
Mobile Push Notifications, Amazon Alexa, Google Home and thousands of other IoT, Home Automation and Web services.

<img src="https://user-images.githubusercontent.com/2234901/144923949-7c5e3f97-fa13-404a-86d1-ac75c828daca.png" width="400"/>

First, create an account with IFTTT and navigate to the Webhooks app settings. Find your Webhooks API Key in the plugin Documentation. The IFTTT Webhook callback URL looks like this:
`https://maker.ifttt.com/use/hdKFgtWRJqZDNFbDjudl92RK3LfDzd1tOZO9KRA15kJ`

The Webhooks API Key is just that last cryptic looking part: `hdKFgtWRJqZDNFbDjudl92RK3LfDzd1tOZO9KRA15kJ`

Copy this key and go to your [Ambianic UI](https://ui.ambianic.ai/) app.

## Test IFTTT integration

Connect to the Ambianic Edge device you would like to configure to send notifications. 
Then go to: _Settings > Device Card > Notifications_.

<img src="https://user-images.githubusercontent.com/2234901/144924453-c149b68a-66d1-4056-98f6-44e2ca067c81.png" width="400"/>

<img src="https://user-images.githubusercontent.com/2234901/144924492-e4e795a4-de0c-4e71-9041-0c76e5eda5b6.png" width="400"/>

Paste your key in the _IFTTT Webhooks API Key_ input field and submit the change.

Make sure to turn on the _Notifications On/Off_ switch. It should read _Notifications On_.

Next, click on the _Test_ . If all is well, you should see a check mark indicating Success for a brief moment.

Now go to your IFTTT dashboard and navigate to the [Webhooks Activity Log](https://ifttt.com/activity/service/maker_webhooks). You should see your test notification appear in the log.

<img src="https://user-images.githubusercontent.com/2234901/144924764-aa4418aa-4d01-498a-b3f7-41e5b5b756f2.png" width="400"/>

Notice that notifications sent by Ambianic Edge devices are marketd with the `ambianic` event tag. 
This allows you to filter and route Ambianic notifications separately from other sources of IFTTT Webhook notifications.

<img src="https://user-images.githubusercontent.com/2234901/144926108-4f34dc8a-fe0c-415d-bd83-fbcd4be87f59.png" width="400"/>

## Compose Notification Automations

You are now ready to create an IFTTT applet that receives `ambianic` events and forwards to your favorite notification channels or automation triggers.
See the [IFTTT Webhooks FAQ](https://help.ifttt.com/hc/en-us/articles/115010230347-Webhooks-service-FAQ) for detailed instructions.

If you experience any problems during this process, you can look for help in the 
[Ambianic Community Discussions](https://github.com/ambianic/ambianic-ui/discussions) or the [Public Slack Channel](https://ambianicai.slack.com/join/shared_invite/zt-eosk4tv5-~GR3Sm7ccGbv1R7IEpk7OQ#/).

## Mobile Push Notifications

IFTTT allows FREE mobile push notifications via Webhooks. 
To receive rich push notifications on your mobile device, first [download the free IFTTT app](https://ifttt.com/home) from your respective mobile app store. 

Next, create a new Applet triggered by the Webhooks plugin that sends a rich notification to your IFTTT app. It should look similar to the following screenshots:

<img src="https://user-images.githubusercontent.com/2234901/144926035-7ad46211-8e07-4d62-8c72-5b3e22a32b5f.png" width="400"/>

<img src="https://user-images.githubusercontent.com/2234901/144926069-103cd17e-757d-4f64-bde8-b6ee2a2c2f99.png" width="400"/>

<img src="https://user-images.githubusercontent.com/2234901/144969204-bf9061f7-85a0-4191-b287-84fb4745f1e8.png" width="400"/>

Now you can go to your Ambianic UI app and send another Test notification. Within a few moments you will see an IFTTT alert on your mobile device.

<img src="https://user-images.githubusercontent.com/2234901/144929539-ca7d4395-2b5a-4eec-8c1a-b3d7cf460ebf.png" width="400"/>

To test that it works end to end, walk in front of your Ambianic smart camera device to trigger a person detection. You will see a notification on your mobile device like the one below.

<img src="https://user-images.githubusercontent.com/2234901/144969433-1300560a-4abf-4222-8c01-030188c0440f.png" width="400"/>

When you click on a notification, it will open a browser window that will load and display all the event details as they would normally appear on your device timeline.

<img src="https://user-images.githubusercontent.com/2234901/144929874-f6406c9e-80ff-4f45-af6c-4723756667e1.png" width="400"/>

<img src="https://user-images.githubusercontent.com/2234901/144930022-9211115b-66e6-453f-8321-e548f13939fa.png" width="400"/>

## Email Notifications

Setting up Email notifications is similar to setting up Mobile Push Notifications. You will still use the Webhook plugin as a trigger. For delivery you can select from several options. IFTTT provides a basic Email Action as well as a more flexible GMail Action. The flow with GMail looks as follows:

<img src="https://user-images.githubusercontent.com/2234901/144933066-49a3423f-0a52-4c47-be39-0c83d7936d53.png" width="400"/>

<img src="https://user-images.githubusercontent.com/2234901/144933082-8d88d37f-d170-4b7f-bb64-a11debde1884.png" width="400"/>

<img src="https://user-images.githubusercontent.com/2234901/144933700-bab5ccc7-474b-416c-84e3-bac1f8e0eb56.png" width="400"/>

When an event is triggered for this flow, the result looks like this:

<img src="https://user-images.githubusercontent.com/2234901/144933861-d9d7af55-92ce-4b06-86fc-6af442d33a49.png" width="400"/>

Clicking on the URL has the same effect as opening a mobile push notification as explained in the previous section.

## SMS Notifications

Setting up Email notifications is similar to setting up Mobile Push Notifications and Email Notifications. You will still use the Webhook plugin as a trigger. For delivery you can select from several SMS provider options. Following is the flow via the ClickSend SMS Action.

<img src="https://user-images.githubusercontent.com/2234901/144935267-0326a2f3-13fe-4e6b-b433-85b1d71a9571.png" width="400"/>

<img src="https://user-images.githubusercontent.com/2234901/144935290-52533ba3-26a8-4244-a123-d11a7ab4b1e9.png" width="400"/>

When a notification is triggered, you can check the ClickSend dashboard to confirm. 

<img src="https://user-images.githubusercontent.com/2234901/144935503-659ce1bb-3b42-4788-b677-97cc87ffa47e.png" width="400"/>

The resulting SMS on your mobile phone would look like the example below:

<img src="https://user-images.githubusercontent.com/2234901/144969523-bf9e1462-1098-4f1b-8644-78fc6050b468.png" width="400"/>

_Note_: Sometimes telecom providers (MNOs) delay SMS messages sent via apps up to a few minutes and in some cases hours. Occasionally telecoms choose to drop messages sesnt via apps if they consider them spam. Unfortunately there is no Spam folder that you can check for dropped SMS messages. Your best bet is to try again, talk to ClickSend Support or use an alternative SMS app.

## Troubleshooting

Keep in mind that the URL of the event contains an encrypted version of the device Peer ID to protect your privacy. Only browsers that have previous knowledge of your device will be able to open the event. In other words if you have used the Ambianic PWA in the browser before and added the device there, the browser will be able to decrypt the event notification URL and fetch all details from the device.

However if you use a different browser to open notifications, or if the URL accidentally ended up in the hands of an unintended user, the PWA will display an error message.

<img src="https://user-images.githubusercontent.com/2234901/144930563-b2965755-1e9b-4cfb-87b0-b771dd02338a.png" width="400"/>

When that happens, you can verify that you are in the browser used to add the devices, by navigating to _Settings < My Devices_ You will likely see an emty list.

<img src="https://user-images.githubusercontent.com/2234901/144931841-8a51a19d-73e0-43e8-bfa8-98bf50390a80.png" width="400"/>

You can either add the device to this browser or alternatively copy and paste the notification URL to the browser which you normally use for the Ambianic PWA and already has the alerting Ambianic Edge device saved.

<img src="https://user-images.githubusercontent.com/2234901/144932094-52d72fee-e139-40c0-86ba-7d7cf6794650.png" width="400"/>

## Advanced Notification Settings

Ambianic Edge devices can be also configured to send notifications via [many other services](https://github.com/caronc/apprise/wiki). 
See the [Advanced Notifications section](configure.md#notification-settings) of the Ambianic Edge configuration guide for more details.
