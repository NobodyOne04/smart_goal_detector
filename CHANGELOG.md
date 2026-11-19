# Changelog

## Unreleased

### Added
- CLAUDE.md, changelog and ADRs: how the detector is built and why

### Fixed
- The README and the notebook named the `chatty-bot` Jupyter kernel; they now use `smart-goal-detector`

### Removed
- torch, transformers and accelerate: listed as dependencies but never used (several GB to install)
