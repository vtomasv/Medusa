# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Testing
- **Run all syntax checks (Py3 local, Py2 Docker):** `./tests/run_agent_code_syntax.sh`
- **Run only Python 3 syntax checks:** `./tests/run_agent_code_syntax.sh py3`
- **Run only Python 2.7 syntax checks:** `./tests/run_agent_code_syntax.sh py2`

## Architecture Overview

Medusa is a cross-platform agent for the Mythic C2 framework, supporting both Python 2.7 and 3.8.

### Agent Structure
The agent uses a "fragmented" architecture where capabilities are spliced into the agent at build time by `builder.py`.

- **Agent Core:** Located in `Payload_Type/medusa/medusa/agent_code/base_agent`. Contains the base logic and transport layers (HTTP, Azure Blob).
- **Agent Commands:** Located in `Payload_Type/medusa/medusa/agent_code`. 
    - Files ending in `.py` are version-agnostic.
    - Files ending in `.py2` are for Python 2.7.
    - Files ending in `.py3` are for Python 3.
    - These files contain indented method definitions intended to be injected into the main agent class.
- **Mythic Integration:** The `Payload_Type/medusa/medusa/mythic` directory contains the necessary integration code for Mythic, including agent functions and browser scripts.

### Key Mechanisms
- **Dynamic Loading:** Commands can be loaded/unloaded at runtime via the `load` and `unload` commands to minimize the agent's on-disk footprint.
- **Memory-Only Modules:** Supports loading zipped Python modules directly into memory.
- **Threading:** Long-running tasks (e.g., uploads/downloads) run in separate threads and implement a "stop check" to allow cancellation via `jobkill`.

### File Layout
- `Payload_Type/medusa/`: Main agent logic and build configuration.
- `tests/`: Syntax validation scripts.
- `documentation-payload/medusa/`: Comprehensive documentation for commands and OPSEC.
