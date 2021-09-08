# End to end manual testing

In addition to executing automated unit tests for each project module and integrated tests across modules, 
we occasionally need to manually walk through the end user experience with the system end to end. 
Manual testing sometimes leads to discovering new issues that can be then covered with new automated tests.

Following is the recommended way to manually test an Ambianic system end to end in development mode, but as close as possible to realistic conditions.

First, start the Ambianic UI and Ambianic Edge gitpod environments. If you haven't done this before, then follow [these instructions](https://docs.ambianic.ai/developers/development-environment/).

Next, go to the Ambianic Edge dev environment and make sure that all main modules are running:
1. Core Monitoring and Inference Engine
1. REST API server (fastapi)
1. WebRTC - HTTP proxy

In your main workspace directory you should see a file named `.peerjsrc` with a contents similar to this:
```
{"peerId": "77d4b8b4-bc2a-451a-9bed-28f07c3e1abc"}
```

Copy to the system clipboard the peerId value from this json formatted file. Just the `77d4b8b4-bc2a-451a-9bed-28f07c3e1abc` part. 
Notice that this value is a randomly generated [UUID4](https://en.wikipedia.org/wiki/Universally_unique_identifier#Version_4_(random)) which will be unique for your environment.

![Screen Shot 2021-09-07 at 7 33 11 PM](https://user-images.githubusercontent.com/2234901/132426954-9a52ee0c-14ed-47bb-be57-227e3f6e16fd.png)

Next, go to the Ambianic UI environment, open a browser window pointing to port 8080 of the gitpod environment serving the Ambianic PWA on nodejs in dev mode.

![Screen Shot 2021-09-07 at 4 48 19 PM](https://user-images.githubusercontent.com/2234901/132427845-0c74edae-22c1-405c-a9e9-2c56375773e7.png)

Click on _Begin Setup_ then on the next page click _Skip_ in the upper right corner of the app.

![Screen Shot 2021-09-07 at 7 38 33 PM](https://user-images.githubusercontent.com/2234901/132428090-464f5c71-52b9-4498-8779-d7f1911f631d.png)

On the next page, select Main menu drop down in the Nav Bar and click Settings.

![Screen Shot 2021-09-07 at 7 41 22 PM](https://user-images.githubusercontent.com/2234901/132428307-817493bb-8f0a-47a5-b3e7-31521ad732be.png)

Then scroll down to the section titled "Pair with remote Ambianic Edge device".

![Screen Shot 2021-09-07 at 7 40 43 PM](https://user-images.githubusercontent.com/2234901/132428376-126ed1e2-a385-4548-88e4-21253ccf584e.png)

Paste the peerjs key value that you copied from the Ambianic Edge CDE. Click on Pair Remotely. 
In a few moments you will see snackbar status messages indicating that the UI is connecting to Ambianic Edge followed by a confirmation that connection was established.

![Screen Shot 2021-09-07 at 7 41 08 PM](https://user-images.githubusercontent.com/2234901/132428700-e837f67d-5eaf-427d-80e2-518220de23b5.png)

If you now scroll up to the top of the Settings page you will see the Peer ID and Version of the Ambianic Edge device you are connected to. 
This is in fact a connection to the gitpod CDE running Ambianic Edge in dev mode.

![Screen Shot 2021-09-07 at 7 59 18 PM](https://user-images.githubusercontent.com/2234901/132429510-38d74d36-3961-4e5a-a797-a9f1584d6b14.png)

Notice that we did not have to open any public HTTP/S ports on the Ambianic Edge CDE in order for the UI to access the virtual device. 
This is exactly how the UI and Edge connect in a real world deployment. 
Instead of relying on access to public HTTP/S endpoints, the UI and Edge connect securely and directly via WebRTC DataChannel 
(using [peerfetch](https://github.com/ambianic/peerfetch) - an HTTP over WebRTC proxy).

If you now navigate to the Timeline page, you will see a timeline view. 

![Screen Shot 2021-09-07 at 8 09 16 PM](https://user-images.githubusercontent.com/2234901/132429564-2bc34837-33dc-41cc-a8f3-107c29e9a40a.png)


The source for this view is populated from the video file configured in `dev-config.yaml` in the Ambianic Edge CDE. 
You can change the file with your own recorded video file for testing various detection scenarios.

![Screen Shot 2021-09-07 at 8 09 46 PM](https://user-images.githubusercontent.com/2234901/132429600-4f17fb59-4252-4962-b2cd-5fbc1071b55f.png)

Notice how the `peerfetch` terminal shows a log of HTTP fetch requests and responses from the Ambianic UI instance.

Now you have a fully setup live and realistic system that runs both Ambianic UI and Ambianic Edge in dev mode. 
This setup represents as closely as possible the real world experience of an end user, while allowing you, dear developer, to test, experiment and commit code.
