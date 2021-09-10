
# Development Environment Setup

This page decribes how to setup a development environment for Ambianic.ai repos.

If you plan to push your own code you should [fork](https://docs.github.com/en/get-started/quickstart/fork-a-repo) the original github repository and work from your local copy/fork.
![Screen Shot 2021-09-07 at 10 40 45 AM](https://user-images.githubusercontent.com/2234901/132374705-2b3bdf7c-a08e-41aa-8f61-9594765e60e1.png)

## Gitpod Continuous Development Environment

The easiest way to start a development environment for Ambianic Edge and Ambianic UI is to use the Gitpod Continuous Development Environment. [Gitpod](https://gitpod.io/) is an All-in-browser Continuous Development Environment with user experience similar to VS Code, but running in a virtual cloud container instead of your local machine. We maintain a Gitpod configuration preset for Ambianic Edge and Ambianic UI as part of the upstream git repository, which makes it as easy as a single click to launch a fully configured and ready to use dev environment. Gitpod offers Free No Limits [Professional Open Source accounts](https://www.gitpod.io/docs/professional-open-source) to qualifying projects maintainers.

If you have not used Gitpod before, go ahead and [give it a try](https://www.gitpod.io/#get-started). 

You can launch your gitpod space for your forked repo in one of two ways:

1. Point your browser to an URL composed of the gitpod web app prefix and your forked repo URL. For example: `https://gitpod.io/#https://github.com/MY_GITHUB_ACCOUNT/ambianic-edge`
2. Alternatively, using the [Gitpod browser extension](https://www.gitpod.io/docs/browser-extension/), just hit the green gitpod button on your fork's main page:
![Screen Shot 2021-09-07 at 10 40 45 AM](https://user-images.githubusercontent.com/2234901/132374456-438268ae-d546-4245-9f25-9377460de97e.png)

When your gitpod space starts for your repo, in the background it will install all the required dependencies, pre-commit hooks and other environment settings required to build, run and test the project. You can focus on writing, testing and committing your new code. You can study the gitpod config file (`.gitpod.yml`) located at the root of your repo for details on the virtual dev environment setup steps.

Once your gitpod space starts it will automatically open and run the main modules for the project. You will also see a terminal running the full testsuite to ensure that you environment is in good shape and ready for coding.

### Ambianic Edge with Gitpod

Once your gitpod space for your Ambianic Edge fork is launched, you will see several shell terminals automatically open and run the main modules that Ambinic Edge is composed of:
1. Core sensor monitoring and inference engine.
2. OpenAPI server (fastapi/uvicorn)
3. Peer-to-peer proxy (HTTP over WebRTC)

You will also see a terminal running the full testsuite to ensure that you environment is in good shape and ready for coding.

Screenshots examples for each terminal window follow:

![Screen Shot 2021-09-07 at 4 42 41 PM](https://user-images.githubusercontent.com/2234901/132417263-2d567d82-4575-46b4-94cd-191f81c65000.png)

*Ambianic Edge gitpod space automatically runs the full testsuite in a terminal window.*

<img src="https://user-images.githubusercontent.com/2234901/132417694-7a5b5a62-3300-4563-a3d0-5349ee81d554.png" width=600/>
          
*Gitpod terminal running the Ambianic Edge Core monitoring and inference engine*

<img src="https://user-images.githubusercontent.com/2234901/132417720-5acf146c-013f-44ce-a38c-3c0b7942a88a.png" width=600/>

_Gitpod terminal running fastapi/uvicorn serving the Ambianic Edge REST API (OpenAPI compliant)_

<img src="https://user-images.githubusercontent.com/2234901/132417727-904aaad2-8933-44ae-b28a-ce7e6697fd77.png" width=600/>
          
_Gitpod terminal running WebRTC-HTTP Proxy which allows Ambianic UI to connect to this Ambianic Edge instance_

#### Ambianic Edge Dev Configuration

The Ambainic Edge instance running in your gitpod space is configured by the `dev-config.yaml` file located in the `dev` directory of your workspace.
If you want to modify the dev config and test with custom settings, you should add `dev-config.yaml` to your `.gitignore` file to ensure that you will not commit it back to the upstream repo.

Notice that the default `dev-config.yaml` file is setup to use a local video file as input source to the AI pipelines.
```
  recorded_cam_feed:
    # location of the recorded video file in a gitpod (or local) dev environment
    uri: file:///workspace/ambianic-edge/tests/pipeline/avsource/test2-cam-person1.mkv
    type: video
    # live: true when we want the pipeline to keep looping over the same video file.
    # live: false when we want the pipeline to go through the video file once and stop.
    live: false
```

This configuration allows you to simulate a realistic Ambianic Edge deployment without connecting to a live camera feed. You can change the source to use video file recordings of your choice.

#### Unit Testing Ambianic Edge

We use [`pytest`](https://docs.pytest.org/en/latest/contents.html) to drive the Ambianic Edge testsuite. To run the full testsuite, you can go to the terminal window that runs it on launch and manually repeate the command:
```
python3 -m pytest -v --log-cli-level=DEBUG --cov=ambianic --cov-report=term tests/
```

This runs all tests and prints a coverage report in the terminal. Also shows in the terminal all log messages logged by the tested code.

You can use the [pytest "-k"](https://docs.pytest.org/en/6.2.x/example/markers.html#using-k-expr-to-select-tests-based-on-their-name) option to run selected tests. 

See the official [pytest docs](https://docs.pytest.org/en/6.2.x/example/markers.html) for other ways to run selected tests.

### Ambianic UI with Gitpod

Once your gitpod space for your Ambianic UI fork is launched, you will see several shell terminals open and run:

1. Ambinic PWA nodejs service. Gitpod will prompt you to open a browser tab for the PWA. 
2. Browser-sync service which allows you to open in a browser window static files from the project directories.
3. You will also see another shell terminal running the full testsuite to ensure that you environment is in good shape and ready for coding.

Screenshots examples for each terminal window follow:

![screenshot](https://user-images.githubusercontent.com/2234901/132419819-3e3d4f76-7793-47de-8e6a-5cd579d653cd.png)

_Ambianic UI gitpod space automatically runs the full testsuite in a terminal window._

<img src="https://user-images.githubusercontent.com/2234901/132419996-79f49512-0a4c-48fa-8703-6d8761322d1a.png" width=600/>

_Terminal window running the Ambianic PWA as a nodejs service in dev mode._

<img src="https://user-images.githubusercontent.com/2234901/132420081-ef1568cb-25fb-4331-a569-f4f8b0f04735.png" width=600/>

_Terminal window running browser-sync._


#### Unit testing Ambianic UI

We use [Jest](https://jestjs.io/) for unit testing and [Cypress](https://www.cypress.io/) for e2e testing.

To run tests, go to the terminal that runs all tests when the gitpod space launches. Then you can

1. Run all tests with `npm run test`
2. Run unit tests with `npm run test:unit`
3. Run e2e tests with `npm run test:e2e`

Unit tests are located under `tests/unit`

E2E tests are located under `cypress/integration/ambianic-tests`

### End to End System testing

If you'd like to manually test UI and Edge together as a whole system, while running in dev mode in conditions as close as possible to a real world deployment, 
take a look at [this guide](e2e-manual-testing.md).
____________________
____________________


## Local Development Environment

Setting up a local dev environment tends to be more work as compared to Gitpod as it takes away compute resources from your workstation and it requires maintaining multiple virtual environments and containers along with developent tools for different language platforms. As the project evolves, it usually requires some local upkeep to make sure all local tools are up to date and in sync with the latest versions used for the current project build. If this is your preferred method, the following guidleninces are for you.

### Setting Up Ambianic Edge on a Local Worsktation 

To setup the dev environment for Ambianic Edge on your local machine you need as prerequisites:

- a *nix machine also in VM
- docker
- git

Clone or fork the repository at https://github.com/ambianic/ambianic-edge

Now open a terminal and move to the `ambianic-edge` folder.

#### Start the development environment

To start the development container run

```sh
./ambianic-debug.sh
```

This command will download the most up to date version of the base container and mount your local directory into it. Once the setup is completed, you will have a shell prompt from inside the container.

You should see something like that

`$ ./ambianic-debug.sh`

```sh
+++ dirname ./ambianic-debug.sh
++ cd .
++ pwd
++ basename ./ambianic-debug.sh
+ MY_PATH=/home/user/ambianic-edge/ambianic-debug.sh
++ dirname /home/user/ambianic-edge/ambianic-debug.sh
+ MY_DIR=/home/user/ambianic-edge
+ USB_DIR=/dev/bus/usb
+ VIDEO_0=/dev/video0
+ '[' -d /dev/bus/usb ']'
+ USB_ARG='--device /dev/bus/usb'
+ VC_SHARED_LIB=
+ grep -s -q 'Raspberry Pi' /proc/cpuinfo
+ '[' -e /dev/video0 ']'
+ VIDEO_ARG='--device /dev/video0'
+ docker pull ambianic/ambianic-edge:dev
dev: Pulling from ambianic/ambianic-edge
Digest: sha256:1b95614012f7a12abd5604471d81ec2b6dbda6d4aa1a622e69ef1605f9ff6004
Status: Image is up to date for ambianic/ambianic-edge:dev
docker.io/ambianic/ambianic-edge:dev
+ docker run -it --rm --name ambianic-edge-dev --mount type=bind,source=/home/user/ambianic-edge,target=/workspace --privileged --net=host --device /dev/video0 --device /dev/bus/usb ambianic/ambianic-edge:dev
```

The shell will show up 

`root@pc:/opt/ambianic-edge#`

Move to `/workspace` directory where the local code is mounted with 

`cd /workspace/`

#### Starting a development instance

Run `./src/run-dev.sh` to start the development instance of Ambianic.ai from the container. 

#### Running tests

To run the test use 

`./tests/run-tests.sh`

The command will install the required dependencies and scan the  `./tests` folder to execute all the tests found.

When developing is common to narrow the scope of testing to the componentes we are working  on. We can use the pytest commmand directly for that purpose, follows a list of handy command that can be used  during development.

_Note_ ensure to run `./tests/run-tests.sh` at least once to have the python dependencies installed!

```sh
# run all the tests
python3 -m pytest tests/

# run only tests found from a specific file
python3 -m pytest tests/pipeline/ai/test_fall_detect.py 

# run test by name (will match the function name)
python3 -m pytest tests/ -k test_bad_sample_good_sample

# run test by name match (will match the function name, also patially)
python3 -m pytest tests/ -k test_bad_sample_good_sample
# with just a part of it
python3 -m pytest tests/ -k bad_sample_good

# control CLI log level
python3 -m pytest tests/ --log-cli-level DEBUG

```

### Debugging with an IDE

This section contains suggestions to setup a proper development environment for better productivity. Feel free to share yours!

#### MS VS Code

vscode offers a plugin  called [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)  that allow to  connect to a container running on a local (or remote) machine.

This is particularly handy when one needs to use the debugger to inspect the runtime. It can also be used to develop on the raspberry PI from your own PC.
### Setting Up Ambianic Edge on a Local worsktation 

### Setting Up Ambianic UI on a Local Workstation

To build, test and debug Ambianic UI on a local laptop or desktop, you will need [node.js](https://nodejs.org/en/) and git installed.

#### Clone repository
```
git clone https://github.com/ambianic/ambianic-ui.git
cd ambianic-ui
```

#### Install dependencies
```
npm install
```

#### Prepare dev environment

Set up a git pre-commit hook for linting.

```
npm run prepare
```

#### Compiles and hot-reloads for development
```
npm run serve
```

#### Compiles and minifies for production
```
npm run build
```

#### Runs all tests
```
npm run test
```
