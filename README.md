# Reintroduction to Proofs

This Lean game aims to provide an informal introduction to proofs and constructions in dependent type theory, aimed either at someone who is learning to write abstract mathematical proofs for the first time, or at a more experienced player who wishes to be *reintroduced* to proofs in a different formal system.

This game was originally created for a first year seminar course taught at Johns Hopkins University in Fall 2025 with the title "Computer-Verified Proof: a Hands-On Introduction to Interactive Theorem Proving." The original audience was comprised of a dozen students with a wide variety of academic interests, from economics to neuroscience.

Contributions to the game can be made by opening issues, submitting pull requests, or contacting me on the Lean zulip.

Acknowledgments: I am extremely grateful to the folks who built the Lean Game Server and the Game Skeleton Repository, as well as those who helped me solve a variety of problems that arose during the development of this game.

## Running locally

This copy runs on Lean v4.35.0 with [our lean4game fork](https://github.com/elonblonz/lean4game) as the engine. With Docker installed:

```
docker compose -f .devcontainer/docker-compose.yml up
```

then open http://localhost:3000 (the first start downloads Mathlib and builds the game). To run next to another game's container, pick other host ports: `GAME_PORT=3010 RELAY_PORT=8081 docker compose ... up`. Opening the folder in VS Code's "Dev Containers" does the same.

The engine release is pinned in `lakefile.lean` (`lean4gameRef`) and `.devcontainer/` (`LEAN4GAME_REF`); change them together and run `lake update GameServer`. `scripts/check_engine_ref.sh` (also run in CI) checks that they match.
