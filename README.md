# autotest
Automated Testing

This Vagrant box provides testing suite with: headless Android emulator and headless Firefox browser.

## Requirements
- Host must have Vagrant and VirtualBox

## Usage
1) vagrant up
2) ssh in and:
```bash
emulator -avd appium -no-skin -no-audio -no-window
```
3) ssh in with another session:
