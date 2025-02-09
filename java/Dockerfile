ARG JAVA_VERSION=8

FROM codingcorp-docker.pkg.coding.net/nocalhost/public/coding-java:latest

ARG USERNAME=nocalhost
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && rm /etc/apt/sources.list.d/jessie-backports.list \
    && rm /etc/apt/sources.list \
    && echo "deb http://mirrors.cloud.tencent.com/ubuntu/ bionic main universe restricted multiverse" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.cloud.tencent.com/ubuntu/ bionic-security main universe restricted multiverse" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.cloud.tencent.com/ubuntu/ bionic-updates main universe restricted multiverse" >> /etc/apt/sources.list \
    && echo "deb http://mirrors.cloud.tencent.com/ubuntu/ bionic-backports main universe restricted multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.cloud.tencent.com/ubuntu/ bionic main universe restricted multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.cloud.tencent.com/ubuntu/ bionic-security main universe restricted multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.cloud.tencent.com/ubuntu/ bionic-updates main universe restricted multiverse" >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.cloud.tencent.com/ubuntu/ bionic-backports main universe restricted multiverse" >> /etc/apt/sources.list \
    && apt-get update \
    && apt install --force-yes -y ca-certificates curl \
           build-essential gcc g++ \
           tzdata zip unzip vim wget \
           git openssh-client zsh bash net-tools \
    && apt-get install --force-yes -y sudo wget \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    #
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.1/zsh-in-docker.sh)" -- \
COPY ./zsh/zsh-in-docker.sh /tmp
RUN chmod +x /tmp/zsh-in-docker.sh && \
    /tmp/zsh-in-docker.sh \
    -a 'SPACESHIP_PROMPT_ADD_NEWLINE="false"' \
    -a 'SPACESHIP_PROMPT_SEPARATE_LINE="false"' \
    -p git \
    -p https://github.com/zsh-users/zsh-autosuggestions \
    -p https://github.com/zsh-users/zsh-completions \
    -p https://github.com/zsh-users/zsh-history-substring-search \
    -p https://github.com/zsh-users/zsh-syntax-highlighting \
    -p 'history-substring-search' \
    -a 'bindkey "\$terminfo[kcuu1]" history-substring-search-up' \
    -a 'bindkey "\$terminfo[kcud1]" history-substring-search-down'

COPY ./zsh/.p10k.zsh /root

ENTRYPOINT [ "/bin/zsh" ]
CMD ["-l"]