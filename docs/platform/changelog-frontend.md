# Changelog

**Application:** Dashboard Server

## unreleased

## 1.7.0 - 2020-06-04

### Added
- Allow sending user invitation mails with password reset link
- Allow user to restore a lost password
- Sidebar on Devices page to filter by Device Type
- Devices page is using "Device Table Config" of the Device Type
- Allow sorting of Devices on Devices page by "name", "address" and "last received"
- Allow org-admin to update device configurations
- Allow org-admin to manage HTTP Integrations
- Add CRON type for configuration values
- Add UI to select predefined CRON expressions in device config
- Allow org-admin to change device app
- Display number of items per list on hardware activation page
- Allow to specify Activation Group on device import
- Add Link from Hardware Activation setup page to Hardware Activation page

### Changed
- Only import non blank config values from CSV
- Add column "Firmware" to import overview

### Fixed
- HTTP Integration "Authentication Method" can be set to "None"
- Fix bug where Lookup option cannot be selected when clicking too slow

## 1.6.0 - 2020-04-09

### Added
- Display more details when sync devices with chirpstack
- New notifications for user actions
- Allow to select organisation when creating a device in configuration -> hardware
- Allow to edit Organisation Name and Logo in Organisation -> Settings
- Allow to import names for devices in Configuration -> Hardware -> Import
- Allow to import config values from separate cols in Configuration -> Hardware -> Import

### Changed
- Use new notifications in organisation -> wMbus Keys
- Change platform title to "IoT Platform" for better white-labeling

### Fixed
- Allow to remove App in configuration -> hardware
- Missing entries in device -> downlinks

## 1.5.1

### Added
- Device synchronisation with Chirpstack

## 1.5.0

### Added
- CSV Import for devices

### Changed
- Rename directory "configs" to "config". Needs update of docker volume path
