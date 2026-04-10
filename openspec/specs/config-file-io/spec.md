# Config File I/O

## Overview

The system provides export and import capabilities so users can persist and restore all configuration parameters (RGV, track, storages) via a JSON file.

---

## Requirements

### Requirement: Export configuration to file
The system SHALL allow users to download all current configuration parameters (RGV, track, storages) as a JSON file.

#### Scenario: Successful export
- **WHEN** user clicks the「匯出」button in the header
- **THEN** the browser downloads a `.json` file containing `version`, `rgv`, `track`, and `storages` fields

#### Scenario: Export filename
- **WHEN** the file is downloaded
- **THEN** the filename SHALL be `rgv-config.json`

---

### Requirement: Import configuration from file
The system SHALL allow users to upload a previously exported JSON file to restore all configuration parameters.

#### Scenario: Successful import
- **WHEN** user clicks the「匯入」button and selects a valid `.json` file
- **THEN** the system restores `rgv`, `track`, and `storages` from the file, replacing current state

#### Scenario: Import does not affect history or results
- **WHEN** a valid config file is imported
- **THEN** `history`, `lastResult`, and `lastFlowResult` SHALL be cleared (not restored from file)

#### Scenario: Invalid file format
- **WHEN** user selects a file that is not valid JSON or missing required top-level keys
- **THEN** the system SHALL display an error message and NOT modify current state

#### Scenario: Missing fields with defaults
- **WHEN** an imported file is missing newer fields (e.g., `width`/`depth` on storages)
- **THEN** the system SHALL apply default values for missing fields, matching the existing migration logic
