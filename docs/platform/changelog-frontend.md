# Changelog

**Application:** Dashboard Server

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