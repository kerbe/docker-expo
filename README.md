# docker-expo
Dockerized Expo application for making cross-platform mobile apps.

# Watchman not included anymore
This docker container used to have facebook/watchman installed, but it seems to have became unneeded.
These days changes in files are detected, even when expo is running inside docker container.
Old Dockerfile has been renamed to `Dockerfile-watchman`, if anyone finds urge to use it.
However facebook/watchman ( https://github.com/facebook/watchman ) has also received updates,
so old dockerfile and my kerbe/watchman docker image doesn't work properly.

## Usage of this Docker container
Expo has changed it's `expo-cli` tooling, and moved from global expo-cli to project based install.
Current best practice is to run `npx expo start`, and using `expo-cli` gives warning. 
This docker container is able to work in both modes, examples given below.
### Old expo-cli way
You pull this container, and then you have expo-cli in your possession. You can
either init your project with container, or with expo-cli installed on your 
computer directly.

Running expo development environment is done like this (command works in WSL, adjust
accordingly if you use PowerShell or CMD while in Windows):
```
docker run -it --rm -p 19000:19000 -p 19001:19001 -v "$PWD:/app" \
-e REACT_NATIVE_PACKAGER_HOSTNAME=192.168.1.101 \
--name=expo kerbe/expo start
```

Here port 19000 is for actual application port, which needs to be open for your mobile.
Port 19001 is default Metro Bundler port.

Set REACT_NATIVE_PACKAGER_HOSTNAME to be your local IP address. If that is not set,
Expo will bind to Docker container local IP. That IP isn't accessible for your 
mobile phone, even if it is in same WLAN.

Setting name for container makes it easier to work with further docker commands.

### New npx expo way
Create small runner script in your application project root, named `apprunner.sh`:
```
#!/bin/bash
npx expo start
```

Then give it run permissions: `chmod +x ./apprunner.sh`

Now you can overwrite default entrypoint with following:
```
docker run -it --rm -p 19000:19000 -p 19001:19001 -v "$PWD:/app" \
-e REACT_NATIVE_PACKAGER_HOSTNAME=192.168.1.101 \
--name=expo --entrypoint "./apprunner.sh" kerbe/expo
```

Rest of the info from old expo-cli way applies.

## Contributing
Having problems with this image? Want to improve it somehow? Open issue or pull request!
