
# Development Environment

This page decribes how to setup a development environment for Ambianic.ai

## Edge

To setup the  environment for the Edge you need as prerequisites:

- docker
- git

Start by forking the repository and cloning it

Go to https://github.com/ambianic/ambianic-edge and hit the __Fork__ button. This will create a copy of the `ambianic-edge` codebase under your account.

Now go to the __Code__ button and copy the repository URI. 

As an example using the git CLI you can run (change your username first!)

```sh
git clone https://github.com/<your username>/ambianic-edge.git
```

but you can use any other git client.

Now open a terminal and move to the `ambianic-edge` folder.

### Start the development environment

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
+ MY_PATH=/home/l/git/github.com/ambianicai/ambianic-edge/ambianic-debug.sh
++ dirname /home/l/git/github.com/ambianicai/ambianic-edge/ambianic-debug.sh
+ MY_DIR=/home/l/git/github.com/ambianicai/ambianic-edge
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
+ docker run -it --rm --name ambianic-edge-dev --mount type=bind,source=/home/l/git/github.com/ambianicai/ambianic-edge,target=/workspace --privileged --net=host --device /dev/video0 --device /dev/bus/usb ambianic/ambianic-edge:dev
```

The shell will show up 

`root@alf:/opt/ambianic-edge#`

Move to `/workspace` directory where the local code is mounted with 

`cd /workspace/`

### Starting a development instance

Run `./src/run-dev.sh` to start the development instance of Ambianic.ai from the container. 

### Running tests

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

