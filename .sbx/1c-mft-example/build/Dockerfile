ARG __alpine_version=3.24
FROM alpine:${__alpine_version}

### Part 1 - install OS packages

# We do not agree with this hadolint rule for Alpine or any other distro that only works with the latest version of a package.
# hadolint ignore=DL3018
RUN apk update --no-cache \
    && apk upgrade --no-cache \
    && apk add --no-cache \
      curl \
      git \
      lazygit \
      neovim \
      neovim-doc \
      openssh-client \
      ripgrep \
      shellcheck \
      shunit2 \
      yazi


### Part 2 - create non root user and their context
### Note: arrange uid/gid to match your host needs
ARG __gid=1001
ARG __gname=mftexample
ARG __uid=1001
ARG __uname=mftexample


# this folder is expected to receive a mount for the user home
## expect user home to be a volume
ARG __mft_user_home=/home/${__uname}
## expect repo git directory to be a bind mount
ARG __git_repo_home=/repo/1c-mft-hybrid-example

RUN { getent group ${__gid} >/dev/null 2>&1 \
        || addgroup -g ${__gid} ${__gname}; } \
    && { getent passwd ${__uid} >/dev/null 2>&1 \
        || adduser -u ${__uid} -g ${__gid} -D -h ${__mft_user_home} ${__uname}; } \
    && mkdir -p \
      "${__mft_user_home}" \
      "${__git_repo_home}" \
    && rm -rf /tmp/* \
    && chown ${__uid}:${__gid} \
        "${__mft_user_home}" \
        "${__git_repo_home}" \
        /tmp
USER ${__uname}

RUN git config --global --add safe.directory "${__git_repo_home}"

WORKDIR "${__git_repo_home}"

VOLUME \
  "${__mft_user_home}" \
  "${__git_repo_home}" \
  /tmp

# Note: as we are running with a read-only root FS, /tmp needs to have a special regimen and be mounted.