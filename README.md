# docker-expo
Dockerized Expo application for making cross-platform mobile apps. This docker 
container has facebook/watchman ( https://github.com/facebook/watchman ) installedi
as well. Watchman is used to detect changes in files, and doing automatic code
reload / recompile in Expo.

I haven't tested this docker container on Linux or Mac environment, I have developed
this for my Win10 Pro environment. For hot reloading you also need extra application
to transfer filechange notifications to container. This can be achieved for example with
either of these:
  * https://github.com/zippoxer/docker-windows-volume-watcher
  * https://github.com/merofeev/docker-windows-volume-watcher

I have used myself first one, which is written in Go.

## Usage of this Docker container
You pull this container, and then you have expo-cli in your possession. You can
either init your project with container, or with expo-cli installed on your 
computer directly.

Running expo development environment is done like this (command works in WSL, adjust
accordingly if you use PowerShell or CMD while in Windows):
```
docker run -it --rm -p 19000:19000 -p 19001:19001 -p 19002:19002 -v "$PWD:/app" \
-e REACT_NATIVE_PACKAGER_HOSTNAME=192.168.1.101 \
-e EXPO_DEVTOOLS_LISTEN_ADDRESS=0.0.0.0 \
--name=expo kerbe/expo start
```

Here port 19000 is for actual application port, which needs to be open for your mobile.
Port 19001 is default Metro Bundler port.
Port 19002 is for Expo Devtools, accessible by browser.
Automatic browser opening doesn't obviously work, but you can open it yourself in your local IP.

Set REACT_NATIVE_PACKAGER_HOSTNAME to be your local IP address. If that is not set,
Expo will bind to Docker container local IP. That IP isn't accessible for your 
mobile phone, even if it is in same WLAN.

Set EXPO_DEVTOOLS_LISTEN_ADDRESS to 0.0.0.0. Current version of expo-cli listens only 127.0.0.1,
which doesn't work for Docker environment. Those forwarded ports need to be listened by
containers internal ip, so we listen all addresses inside container with 0.0.0.0.

Setting name for container makes it easier to work with docker-windows-volume-watcher helpers.

## Contributing
Having problems with this image? Want to improve it somehow? Open issue or pull request!
