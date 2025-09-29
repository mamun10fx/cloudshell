# Use a standard base image from Microsoft
FROM mcr.microsoft.com/devcontainers/base:bullseye

# Define arguments for tool versions to easily update them later
ARG ANDROID_CMD_TOOLS_VERSION=11076708
ARG ANDROID_PLATFORM_VERSION=34
ARG ANDROID_BUILD_TOOLS_VERSION=34.0.0

# Set environment variables for the Android SDK
ENV ANDROID_HOME="/opt/android-sdk"
ENV PATH="$PATH:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools"

# Switch to root user to install packages
USER root

# Install dependencies and OpenJDK 17
RUN apt-get update &&     apt-get install -y wget unzip openjdk-17-jdk &&     apt-get clean &&     rm -rf /var/lib/apt/lists/*

# Download and install the Android SDK command-line tools
RUN mkdir -p ${ANDROID_HOME}/cmdline-tools &&     wget -q https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_CMD_TOOLS_VERSION}_latest.zip -O /tmp/cmd-tools.zip &&     unzip -q /tmp/cmd-tools.zip -d /opt/android-sdk/cmdline-tools &&     mv /opt/android-sdk/cmdline-tools/cmdline-tools /opt/android-sdk/cmdline-tools/latest &&     rm /tmp/cmd-tools.zip

# Accept SDK licenses and install required packages
RUN yes | sdkmanager --licenses > /dev/null &&     sdkmanager "platform-tools" "platforms;android-${ANDROID_PLATFORM_VERSION}" "build-tools;${ANDROID_BUILD_TOOLS_VERSION}"

# Change ownership of the SDK directory to the vscode user
RUN chown -R vscode:vscode ${ANDROID_HOME}

# Switch back to the non-root 'vscode' user
USER vscode
