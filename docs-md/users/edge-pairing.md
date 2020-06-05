# This is an overview of the connection flow between Ambianic UI and Ambianic Edge

Upon starting the Ambianic Web UI, it will automatically start searching for an Ambianic Edge on your own local network. 

<img src="../assets/images/home-screen.png" />

Either press `VIEW TIMELINE` to see the data from your own Edge, or press `SETTINGS` if you wish to connect to someone else's Edge (remote connection) or see details about your own Edge. 

If you are not connected to a network, you will see a card showing that the UI is looking for an Edge on your local network.

<img src="../assets/images/pairing-screen.png" />

Once connected, it will change to:

<img src="../assets/images/ambianic-connection-details.png" width='350' height='350' />

If you don't want to be connected to your own netwrok, for example you want to connect to your parents Edge, you can scroll down to `Pair with Remote Ambianic Edge device` and enter the PeerJS ID you get *from* them.

<img src="../assets/images/ambianic-remote-connect.png" width='850' height='275' />

Enter the Peer JS ID in the input field and press `PAIR REMOTELY`. 

Once connected, click on the link `Timeline` to see the Edge's data.

When done, go back to `Settings` and use `DISCOVER ON LOCAL NETWORK`

<img src="../assets/images/ambianic-local-connection.png" width='850' height='200' />

This will make sure you get disconnected from the remote Edge and connect back to your own local network Edge.
