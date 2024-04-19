Prerequisites/dependencies:

```bash
sudo dnf install gcc gcc-c++ openssl-devel bzip2-devel libffi-devel zlib-devel cmake libevdev-devel systemd-devel yaml-cpp-devel boost-devel
```

Clone the source to build:

```bash
git clone https://gitlab.com/interception/linux/tools.git

git clone https://gitlab.com/interception/linux/plugins/caps2esc.git
```

Build both repositories using:

```bash
$ cd caps2esc || cd interception
$ cmake -B build -DCMAKE_BUILD_TYPE=Release
$ cmake --build build


# Then copy the binaries to /usr/bin

sudo cp build/caps2esc /usr/bin
sudo cp build/interception /usr/bin
...
```

Copy the provided `udevmon.service` file from the *interception* repository into `/etc/systemd/system/`. Just make sure that the file properly refers to the location of the .yaml file
```bash
sudo cp ./interception/udevmon.service /etc/systemd/system/
```

In /etc/udevmon.yaml, paste this:

```yaml
- JOB: "intercept -g $DEVNODE | caps2esc | uinput -d $DEVNODE"
  DEVICE:
    EVENTS:
      EV_KEY: [KEY_CAPSLOCK, KEY_ESC]
```

### To test:
Open a terminal, hold Caps Lock + L, this should clear the terminal
