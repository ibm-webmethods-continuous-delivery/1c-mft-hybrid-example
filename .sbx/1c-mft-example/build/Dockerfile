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
# For rootless containers use the root group for RW permissions on the bind mounts
# on the host add RW permissions for the user group on the entire folder
ARG __consider_additional_group=false
ARG __additional_group_id=0


# this folder is expected to receive a mount for the user home
## expect user home to be a volume
ARG __mft_user_home=/home/${__uname}
## expect repo git directory to be a bind mount
ARG __git_repo_home=/repo/1c-mft-hybrid-example

RUN group_name="${__gname}" \
    && if ! getent group "${__gid}" >/dev/null 2>&1; then \
         addgroup -g "${__gid}" "${__gname}" 2>/dev/null || addgroup "${__gname}"; \
       else \
         group_name=$(getent group "${__gid}" | cut -d: -f1); \
       fi \
    && if ! getent passwd "${__uid}" >/dev/null 2>&1; then \
         adduser -u "${__uid}" -G "${group_name}" -D -h "${__mft_user_home}" "${__uname}" 2>/dev/null \
         || adduser -G "${group_name}" -D -h "${__mft_user_home}" "${__uname}"; \
       fi \
    && if [ "${__consider_additional_group}" = "true" ]; then \
         if ! getent group "${__additional_group_id}" >/dev/null 2>&1; then \
           addgroup -g "${__additional_group_id}" "group_${__additional_group_id}" 2>/dev/null \
           || addgroup "group_${__additional_group_id}"; \
         fi; \
         addgroup_name=$(getent group "${__additional_group_id}" | cut -d: -f1); \
         addgroup "${__uname}" "${addgroup_name}"; \
       fi \
    && mkdir -p \
      "${__mft_user_home}" \
      "${__git_repo_home}" \
    && rm -rf /tmp/* \
    && chown -R "${__uname}:${group_name}" \
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
