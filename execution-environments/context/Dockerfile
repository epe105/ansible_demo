ARG EE_BASE_IMAGE="quay.io/ansible/ansible-runner:latest"
ARG EE_BUILDER_IMAGE="quay.io/ansible/ansible-builder:latest"
ARG PYCMD="/usr/bin/python3"
ARG PKGMGR_PRESERVE_CACHE=""
ARG ANSIBLE_GALAXY_CLI_COLLECTION_OPTS=""
ARG ANSIBLE_GALAXY_CLI_ROLE_OPTS=""

# Base build stage
FROM $EE_BASE_IMAGE AS base
USER root
ENV PIP_BREAK_SYSTEM_PACKAGES=1
ARG EE_BASE_IMAGE
ARG EE_BUILDER_IMAGE
ARG PYCMD
ARG PKGMGR_PRESERVE_CACHE
ARG ANSIBLE_GALAXY_CLI_COLLECTION_OPTS
ARG ANSIBLE_GALAXY_CLI_ROLE_OPTS

COPY _build/scripts/ /output/scripts/
COPY _build/scripts/entrypoint /opt/builder/bin/entrypoint

# Builder build stage
FROM $EE_BUILDER_IMAGE AS builder
ENV PIP_BREAK_SYSTEM_PACKAGES=1
WORKDIR /build
ARG EE_BASE_IMAGE
ARG EE_BUILDER_IMAGE
ARG PYCMD
ARG PKGMGR_PRESERVE_CACHE
ARG ANSIBLE_GALAXY_CLI_COLLECTION_OPTS
ARG ANSIBLE_GALAXY_CLI_ROLE_OPTS

COPY _build/scripts/pip_install /output/scripts/pip_install
RUN /output/scripts/pip_install $PYCMD

# Final build stage
FROM base AS final
ENV PIP_BREAK_SYSTEM_PACKAGES=1
ARG EE_BASE_IMAGE
ARG EE_BUILDER_IMAGE
ARG PYCMD
ARG PKGMGR_PRESERVE_CACHE
ARG ANSIBLE_GALAXY_CLI_COLLECTION_OPTS
ARG ANSIBLE_GALAXY_CLI_ROLE_OPTS

COPY --from=builder /output/ /output/
RUN /output/scripts/install-from-bindep && rm -rf /output/wheels
RUN rm -rf /output
LABEL ansible-execution-environment=true
