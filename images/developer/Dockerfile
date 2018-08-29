FROM flisboac/ws-base

# NOTE: Additional environment variables should be set up 
# by shell scripts in your (volume-mounted) home folder.
# "$USER_SETUP_SCRIPT" is an executable that will be called
# once in the image build.

ENV WS_AT="guest"
ENV WS_HOME="/usr/local/share/ws"
ENV WS_WORKSPACE="${WS_WORKSPACE:?WS_WORKSPACE not given.}"
ENV WS_WORKSPACE_HOME="/workspace"

ENV WS_UNAME="${WS_UNAME:-developer}"
ENV WS_UID="${WS_UID:-1000}"
ENV WS_UHOME="/home/$WS_UNAME"
ENV WS_SHELL="${WS_SHELL:-/bin/sh}"
ENV WS_UBRIEF="${WS_UBRIEF-Software Developer}"
ENV WS_GNAME="${WS_GNAME:-$WS_UNAME}"
ENV WS_GID="${WS_GNAME:-$WS_UID}"

# ---

VOLUME "$WS_WORKSPACE_HOME"
VOLUME "$WS_HOME"
ENV PATH="$WS_HOME/bin:$PATH"

# Create the developer user
RUN groupadd -r "$WS_GNAME" -g "$WS_GID" \
    && useradd -u "$WS_UID" -r -g "$WS_GNAME" -d "$WS_UHOME" -s "$WS_SHELL" -c "WS_UBRIEF" "$WS_UNAME"

# Add sudo permissions
RUN echo "$WS_UNAME ALL=(ALL) NOPASSWD:ALL" > "/etc/sudoers.d/$WS_UNAME"

# Install internal WS scripts
RUN mkdir /root/ws-scripts
ADD scripts/* /root/ws-scripts
RUN echo "#$WS_SHELL\n[ "$?" -gt 0 ] && exec \"$@\"\n$WS_SHELL" > /root/ws-scripts/ws-run
RUN chmod a+x /root/ws-scripts/*

# Run first-time setup script
WORKDIR "$WS_WORKSPACE_HOME"
RUN /root/ws-scripts/ws-build

# ---

WORKDIR "$WS_WORKSPACE_HOME"
USER "$WS_UNAME"
ENTRYPOINT [ "/root/ws-scripts/ws-run" ]
CMD [ "bash" ]
