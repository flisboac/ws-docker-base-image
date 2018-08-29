FROM flisboac/ws-base

# NOTE: Additional environment variables should be set up 
# by shell scripts in your (volume-mounted) home folder.
# "$USER_SETUP_SCRIPT" is an executable that will be called
# once in the image build.

ENV WS_AT="guest"
ENV WS_HOME="/usr/local/share/ws"

ENV WS_USER_NAME="${WS_USER_NAME:-developer}"
ENV WS_USER_ID="${WS_USER_ID:-1000}"
ENV WS_USER_HOME="${WS_USER_HOME:-/home/$WS_USER_NAME}"
ENV WS_USER_BUILD_SCRIPT="${WS_USER_BUILD_SCRIPT:-$WS_USER_HOME/.wsbuild}"
ENV WS_USER_SHELL="${WS_USER_SHELL:-/bin/sh}"
ENV WS_GROUP_NAME="${WS_GROUP_NAME:-$WS_USER_NAME}"
ENV WS_GROUP_ID="${WS_GROUP_NAME:-$WS_USER_ID}"

# Setup WS tool
VOLUME "$WS_HOME"
ENV PATH="$WS_HOME/cmd:$WS_HOME/bin:$PATH"

# Create the developer user
VOLUME "$WS_USER_HOME"
RUN groupadd -r "$WS_GROUP_NAME" -g "$WS_GROUP_ID" \
    && useradd -u "$WS_USER_ID" -r -g "$WS_GROUP_NAME" -d "$WS_USER_HOME" -s "$WS_USER_SHELL" -c "Software Developer" "$WS_USER_NAME"

# Add sudo permissions
RUN echo "$WS_USER_NAME ALL=(ALL) NOPASSWD:ALL" > "/etc/sudoers.d/$WS_USER_NAME"

# Run first-time setup script
WORKDIR "$WS_USER_HOME"
USER "$WS_USER_NAME"
RUN { [ -f "$WS_USER_BUILD_SCRIPT" ] && [ -x "$WS_USER_BUILD_SCRIPT" ]; } && "$WS_USER_BUILD_SCRIPT"


ENTRYPOINT [ "$WS_USER_SHELL" ]
