# syntax=docker/dockerfile:1.4

# -----------------------------------------------------------------------------
# Base
# -----------------------------------------------------------------------------

FROM --platform=linux/amd64 ubuntu:24.04 AS base

SHELL [ "/usr/bin/env", "/bin/bash", "-c" ]

RUN apt-get update \
    && apt-get install \
    -y --no-install-recommends \
    gcc-13 build-essential qemu-system expect  \
    && apt-get autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# -----------------------------------------------------------------------------
# Build
# -----------------------------------------------------------------------------

FROM base AS build

ARG UID=10001
RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "/nonexistent" \
    --shell "/sbin/nologin" \
    --no-create-home \
    --uid "${UID}" \
    appuser
USER appuser

# -----------------------------------------------------------------------------
# Final
# -----------------------------------------------------------------------------

FROM build AS final

WORKDIR /maman01
COPY ./xv6-01 /maman01
ENTRYPOINT [ "/bin/bash" ]
