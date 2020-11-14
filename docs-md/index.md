
[![Ambianic logo](https://avatars2.githubusercontent.com/u/52052162?s=200&v=4)](https://ambianic.ai)
 &nbsp; 
 &nbsp; 
 
 
<a href="https://landscape.lfai.foundation/format=card-mode&selected=ambianic">
  <img src="https://github.com/lfai/artwork/raw/master/lfaidata-assets/lf-member/associate/lf_mem_asso.png"  height="200" style="display:inline;vertical-align:middle;padding:2%">   
</a>
<a href="https://twitter.com/TensorFlow/status/1291071490062983172?s=20">
  <img src="https://pbs.twimg.com/profile_banners/1195860619284664320/1596827858/1500x500"  height="200" style="display:inline;vertical-align:middle;padding:2%">   
</a>

# Helpful AI for Home and Business Automation

Ambianic is an [award winning](https://blog.ambianic.ai/2020/11/05/awards.html) ambient intelligence project. Ambianic's mission is to make our homes and workspaces a little cozier by providing helpful and actionable suggestions. Ambianic is an Open Source Ambient Intelligence platform that puts local control and privacy first. It enables users to train and share custom AI models without compromising privacy.

[View on Github](https://github.com/ambianic/ambianic-core)

Project background:
* This blog post covers the initial use case and motivation behind the project: [Cut The Cloud Strings Attached to Plug-and-Play Surveillance Cameras](https://blog.ambianic.ai/2020/02/05/pnp.html)
* This WebRTCHacks article goes deeper into the technical design decisions for the project: [Private Home Surveillance with the WebRTC DataChannel](https://webrtchacks.com/private-home-surveillance-with-the-webrtc-datachannel/)

## The 5 Whys

What's the root cause for Ambianic.ai to exist? Below is a
[5 Whys](https://en.wikipedia.org/wiki/Five_whys) diagram that
tries to provide objective answers:

[![Ambianic 5 Whys](assets/diagrams/ambianic-5whys.svg)](https://www.lucidchart.com/invitations/accept/5e0e2084-0d50-499b-afa3-7bea9f82d1f9)

Needless to say there are
subjective reasons which are equally if not more influential for the existence
of this project such as basic human excitement to serve a bigger purpose
via open source AI.

## User Journey

Ambianic's roadmap is inspired by user stories and community feedback.
The following diagram illustrates an example user journey.

[![Ambianic example user journey](assets/diagrams/ambianic-example-user-journey.svg)](https://www.lucidchart.com/invitations/accept/b350d806-3c50-46cb-a39a-98b766f1c4af)


User journeys help us align on the bigger picture and segue into
agile development constructs such as user stories and sprints.
More user journeys will be added over time as the project evolves. Some of the
candidate topics include:

- **Home Automation**
    - Turn traditional door locks into smart locks with Face Recognition.
    - Alert parents if a crying toddler is left unattended for more than 15 minutes.
    - Raise an alert if a baby is seated near a car door without child lock enabled while in motion.

- **Business Automation**
    - Prevent accidents by alerting drivers who act sleepy or distracted.
    - Make sure that a factory floor position is not left unattended for more than 15 minutes.
    - Recognize presence of unauthorized people in a restricted access work area.

## User - System Interactions

Users interact with the system in two phases:

1. First Time Installation
2. Consequent App Engagements

The following diagram illustrates the high level user - system interactions.

[![Ambianic User - System Interactions](assets/diagrams/ambianic-user-system-interactions.svg)](https://www.lucidchart.com/invitations/accept/78d403ce-ebf5-45b3-a4c3-8b89679b0667)

## User Interface Flow

The User Interface is centered around three main activities:

1. Setup Ambianic Edge to communicate with smart home devices: sensors, cameras, microphones, lights, door locks, and others.
2. Design flows to automatically observe sensors and make helpful recommendations.
3. Review event timeline, alerts and actionable recommendations.

Ambianic UI is an Offline-First
[PWA](https://en.wikipedia.org/wiki/Progressive_web_applications)
(Progressive Web Application).
PWAs work in any browser, but "app-like" with features such as being
independent of connectivity, install to home screen, and push messaging depend
on browser support.

Ambianic UI does not assume that the user has constant
broadband internet access. Its built to handle a range of real world scenarios
with low bandwidth or no-Internet access at all when the user may need to
 review Ambianic alerts, events timeline, edit flows and configure edge devices.

Ambianic UI stores data locally on the client device (mobile or desktop) and,
when there’s a network connection,
syncs data to the user's Ambianic server and resolves any data conflicts.
When possible it communicates directly with local Ambianic Edge devices
minimizing network routing overhead.

[![Ambianic User Interface Flow](assets/diagrams/ambianic-user-flow.svg)](https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=ambianic-user-flow#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1BgeZn_ZX6VTag2fA2HLtwJQIqYhFi6LI%26export%3Ddownload)

## Product Design Goals

Our goal is to build a product that is useful out of the box:

 - Less than 15 minutes setup time
 - Less than $75 in hardware costs
   + Primary platform: Raspberry Pi 4 B, 4GB RAM, 32GB SDRAM
 - No coding required to get started
 - Decomposable and hackable for open source developers

## Quick Start

If you would like to try the latest version, follow the steps in the [Quick Start Guide](users/quickstart.md).

## Community Support

If you have questions, ideas or cool projects you'd like to share with the Ambianic team and community, please use the [Ambianic Twitter channel](https://twitter.com/ambianicai).

## Contributors
If you are interested in becoming a contributor to the project, please read the [Contributing](legal/CONTRIBUTING.md) page and follow the steps. Looking forward to hearing from you!

## Sustainability 🌱

* Ambianic is 100% Open Source and funded by its user community.
* 100% Open Source ensures full transparency, privacy and user data ownership.
* By design, there are no other direct or indirect sources of funding that compromise the core values of the project.
* The project [roadmap is open](https://github.com/orgs/ambianic/projects/1) to community feedback and input. 
* As we reach new levels of regular sponsors, we run a community survey to prioritize the next big roadmap feature.
* If you are using the project on a daily basis at home, consider investing in its future success either with your time as a contributor or with a small coffee cup [donation](https://github.com/sponsors/ambianic). Thank you! 🙏
* If you don't like the direction the project is going and your voice is not heard, cancel your support, fork the code and go in a direction that works for you. Such is the balancing power 👮 of open source software. Keeps everyone honest and humble 😌. 


## Responsibilty 📎

Ambianic does not currently operate within a trusted third party network for installation and setup purposes. The Ambianic product is currently a standalone open sourced asset and no liability can be assumed for any decision taken from an individual or group seeking to utilise its benefits. 

Nonetheless, potential and actual users of the system, both direct and indirect, could bear in mind the following advice from Ambianic:

1. Conduct proper research on the utility of the product
2. Set clear goals for product functionality 
3. Try to understand the product's benefits and limitations
4. In the event of uncertainty please use professional level skills for installation and system setup projects
5. Beware of any 'too good to be true' offers from marketeers offering services that relate to this product

Ambianic has been designed to enable a do-it-yourself project although it is recognised that one person's "easy" level of technical expertise is another person's "headache". For anyone attempting their own independent installation project a certain level of knowledge relating to computer software and hardware is assumed as necessary. 

Ambianic is currently an open source asset offered in the spirit of beneficial assistance to help those in need or the families and / or those close to those in need. Any use of Ambianic is done so entirely at the risk of the user/s or user/s' representatives (expressed or deemed) and no responsibility for any adverse consequences through system usage can be assumed by Ambianic.
