FROM kerbe/watchman

USER node

# docker hub needs --unsafe-perm flag to install expo-cli properly
#RUN sudo npm install -g --unsafe-perm expo-cli
RUN sudo yarn global add expo-cli
RUN sudo npm cache clean --force && sudo yarn cache clean

WORKDIR /app

EXPOSE 19000
EXPOSE 19001
EXPOSE 19002

ENTRYPOINT ["expo"]
