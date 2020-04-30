# This is an overview in how the connection stream is running in the UI to Edge

This just describes the flow inside Ambianic-UI, how it connects to Edge
and creates an Edge Room for secure communication

In ChooseEdgeConnection, the user will choose to either connect to a local network or using the Room ID they got from a person which they can connect to.

EdgeConnect will load, but until the backend have authenticated, it will just
show a loading bar.

![Edge Searching][searching]

In the backend, the choise the user made will arrive in pnp.js, in the function `discoverRemotePeerId` which will check if there was a Room ID supplied. If there was, just return that RoomID and head on with connection to Edge in `actions [PEER_AUTHENTICATE]`.

Once connection is established, EdgeConnect will show that we are connected to a certain room.

![Edge Connected][connected]

Once connected, the user can click `Disconnect` which will remove the PeerID making sure the website forgets the Edge ID.

To simplify the flow, a basic sequence diagram showing the flow inside the different files for future debugging and improvement.

![Sequence Diagram][sequence]


[connected]: ../assets/images/connected-screen.PNG
[searching]: ../assets/images/pairing-screen.PNG
[sequence]: ../assets/images/connection-sequence.PNG