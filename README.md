# Brave Browser Container Image 🐳

## Setup

### Step 1: Select or Build Image

Perform **one** of the following steps:
- ```sh
  $ git clone https://github.com/fphammerle/docker-brave-browser
  $ cd docker-brave-browser
  $ exec docker build -t [IMAGE_NAME] .
  ```
- Select a pre-built image at https://hub.docker.com/r/fphammerle/brave-browser/tags<br>
  (e.g., `docker.io/fphammerle/brave-browser:0.2.0-browser1.22.71-amd64`)

### Step 2: Start Dedicated X Server

Choose some arbitrary `[DISPLAY_NUMBER]` and run:
```sh
$ Xephyr -resizeable :[DISPLAY_NUMBER]
# for example:
$ Xephyr -resizeable :1
```

Alternative: Adapt the access rights of your main X server<br>
(cave: `xhost +` is horribly insecure)

### Step 3: Launch Container

```sh
$ exec docker run --name brave_browser --rm --init \
    -e DISPLAY=:[DISPLAY_NUMBER] -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v brave_browser_home:/home/browser --shm-size 1GB \
    --read-only --tmpfs /tmp:size=8k \
    --cap-drop ALL --security-opt no-new-privileges \
    [IMAGE_NAME]
```

Add `--tmpfs /tmp:size=8k` when using `docker`.

### En gros:
```sh
git clone git@github.com:Comarco/docker-brave-browser.git
sudo docker build -t docker.io/fphammerle/brave-browser:0.2.1-browser1.41.96-amd64 .
Xephyr -resizeable -screen 1280x780 :100&
sudo docker run --name brave_browser --rm --init -e DISPLAY=:100 -v /tmp/.X11-unix:/tmp/.X11-unix -v brave_browser_home:/home/browser --shm-size 2GB --read-only --tmpfs /tmp:size=8k     --cap-drop ALL --security-opt no-new-privileges docker.io/fphammerle/brave-browser:0.2.1-browser1.41.96-amd64
``
