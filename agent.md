# Medusa Agent Definition

## Overview
Medusa is a cross-platform agent compatible with Python 3.8 and Python 2.7, designed for the Mythic C2 framework. It focuses on modularity and minimizing on-disk exposure through dynamic loading of capabilities.

## Capabilities
- **Cross-Platform:** Operates on Windows, Linux, and macOS.
- **Dynamic Loading:** Commands can be loaded and unloaded in-memory.
- **Transport Profiles:** Supports HTTP (encrypted and unencrypted) and Azure Blob storage.
- **Python Versioning:** Maintains parallel support for Py2 and Py3 using a fragment-based build system.

## Development Guidelines

### Adding New Functionalities
When adding a new command or modifying existing ones:
1. **Create Agent Code:** Add the command fragment in `Payload_Type/medusa/medusa/agent_code`.
    - Use `.py` for version-agnostic code.
    - Use `.py2` and/or `.py3` if version-specific syntax is required.
    - Ensure the code is indented as it will be spliced into a class.
2. **Implement Mythic Function:** Update `Payload_Type/medusa/medusa/mythic/agent_functions` to define how Mythic interacts with the agent code.
3. **Update Documentation:** Add a corresponding `.md` file in `documentation-payload/medusa/commands` describing the syntax and purpose of the new command.
4. **Verify Syntax:** Run `./tests/run_agent_code_syntax.sh` to ensure the fragment compiles under the target Python version(s).

### Implementation Patterns
- **Threading:** Use threading for long-running tasks.
- **Stop Checks:** Always implement a check for the `stopped` flag in the tasking list to support `jobkill`.
- **Output Updates:** Use `self.sendTaskOutputUpdate(task_id, message)` for long-running tasks to provide real-time feedback.

## Build Process
The agent is built via `builder.py`, which:
1. Selects the base agent version (Py2 or Py3).
2. Collects compatible command fragments based on the selected version.
3. Slices these fragments into the final agent source code.
