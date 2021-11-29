
# Ambianic Notifications

Ambianic Edge can be configured to send instant notifications to a broad range of services.

## UI Notification Settings

Instant notifications can be enabled via the Ambianic UI PWA. Follow the instructions below to setup and test your first notification message.

### Get an IFTTT Webhook API KEY

[IFTTT](https://ifttt.com) is a popular notification hub that provides a convenient way to create automation workflows for SMS, Email, 
Mobile Push Notifications, Amazon Alexa, Google Home and thousands of other IoT, Home Automation and Web services.

First, create an account with IFTTT and navigate to the Webhooks app settings. Find your Webhooks API Key. The IFTTT Webhook callback URL looks like this:
`https://maker.ifttt.com/use/hdKFgtWRJqZDNFbDjudl92RK3LfDzd1tOZO9KRA15kJ`

The Webhooks API Key is just that last cryptic looking part: `hdKFgtWRJqZDNFbDjudl92RK3LfDzd1tOZO9KRA15kJ`

Copy this key and go to your [Ambianic UI](https://ui.ambianic.ai/) app.

### Enter and Test IFTTT integration

Connect to the Ambianic Edge device you would like to configure to send notifications. 
Then go to: _Settings > Device Card > Notifications_.

Paste your key in the _IFTTT Webhooks API Key_ input field and submit the change.

Make sure to turn on the _Notifications On/Off_ switch. It should read _Notifications On_.

Next, click on the _Test_ . If all is well, you should see a check mark indicating Success for a brief moment.

Now go to your IFTTT dashboard and navigate to the [Webhooks Activity Log](https://ifttt.com/activity/service/maker_webhooks). You should see your test notification appear in the log.

Notice that notifications sent by Ambianic Edge devices are marketd with the `ambianic` event tag. 
This allows you to filter and route Ambianic notifications separately from other sources of IFTTT Webhook notifications.

### Compose Notification and Automation Applets

You are now ready to create an IFTTT applet that receives `ambianic` events and forwards to your favorite notification channels or automation triggers.
See the [IFTTT Webhooks FAQ](https://help.ifttt.com/hc/en-us/articles/115010230347-Webhooks-service-FAQ) for detailed instructions.

If you experience any problems during this process, you can look for help in the 
[Ambianic Community Discussions](https://github.com/ambianic/ambianic-ui/discussions) or the [Public Slack Channel](https://ambianicai.slack.com/join/shared_invite/zt-eosk4tv5-~GR3Sm7ccGbv1R7IEpk7OQ#/).

## Advanced Notification Settings

For more advanced notification configuration options, see the [Notifications section](configure.md#notification-settings) of the Ambianic Edge configuration guide.
