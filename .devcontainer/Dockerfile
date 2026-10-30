FROM node:22

ENV ELAN_HOME=/usr/local/elan \
  PATH=/usr/local/elan/bin:$PATH

SHELL ["/bin/bash", "-euxo", "pipefail", "-c"]

COPY ./lean-toolchain /lean-toolchain

# Install the Lean toolchain pinned by `lean-toolchain`.
RUN curl https://raw.githubusercontent.com/leanprover/elan/master/elan-init.sh -sSf \
    | sh -s -- -y --no-modify-path --default-toolchain "$(tr -d '\r' < /lean-toolchain)"; \
  chmod -R a+w $ELAN_HOME; \
  lean --version; \
  lake --version

USER node
WORKDIR /home/node

# Clone the game engine and install its npm dependencies here, so they are cached in the image
# rather than redone per container. This is a release of our engine fork (upstream lean4game
# plus our changes, tags on branch `stg4-ui`); it must match `lean4gameRef` in `lakefile.lean`.
# Set LEAN4GAME_REPO / LEAN4GAME_REF (see docker-compose.yml) to build against another one.
ARG LEAN4GAME_REPO=https://github.com/elonblonz/lean4game.git
ARG LEAN4GAME_REF=stg4-v7
RUN git clone --depth 1 --branch "$LEAN4GAME_REF" "$LEAN4GAME_REPO" lean4game \
  && cd lean4game && npm ci

# Mount point for the `.lake` named volume (see docker-compose.yml). Creating it here
# makes the volume owned by `node` instead of `root`.
RUN mkdir -p /home/node/game/.lake
# Keep Mathlib's downloaded cache in that volume too, so restarts don't download it again.
ENV MATHLIB_CACHE_DIR=/home/node/game/.lake/mathlib-cache

WORKDIR /home/node/game
