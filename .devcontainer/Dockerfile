FROM debian:trixie-slim
# https://adoptium.net/installation/linux/

ENV ANDROID_HOME=/sdk
ENV JAVA_HOME=/usr/lib/jvm/temurin-8-jdk-amd64
ENV PATH=$PATH:$JAVA_HOME/bin:$ANDROID_HOME/platform-tools:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/build-tools/20.0.0
ENV ADB_SERVER_SOCKET=tcp:localhost:5037
WORKDIR /sdk
RUN apt-get update && \
    apt-get install -y --no-install-recommends ca-certificates wget apt-transport-https gpg && \
    wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public \
        | gpg --dearmor \
        | tee /etc/apt/trusted.gpg.d/adoptium.gpg > /dev/null && \
    echo "deb https://packages.adoptium.net/artifactory/deb trixie main" \
        > /etc/apt/sources.list.d/adoptium.list && \
    dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        temurin-8-jdk \
        unzip \
        libc6:i386 libstdc++6:i386 zlib1g:i386 && \
    rm -rf /var/lib/apt/lists/*
RUN wget \
    https://dl.google.com/android/repository/commandlinetools-linux-8092744_latest.zip \
    -O commandline.zip && \
    unzip -q commandline.zip && \
    rm -f commandline.zip && \
    mv cmdline-tools _temp && \
    mkdir -p cmdline-tools/latest && \
    mv _temp/* cmdline-tools/latest && \
    rm -rf _temp
RUN yes | ${ANDROID_HOME}/cmdline-tools/latest/bin/sdkmanager --licenses && \
    ${ANDROID_HOME}/cmdline-tools/latest/bin/sdkmanager --install "build-tools;20.0.0" "platforms;android-19" "platform-tools" "add-ons;addon-google_gdk-google-19"
WORKDIR /workspace