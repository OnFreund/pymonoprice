## Status
[![Build Status](https://github.com/OnFreund/pymonoprice/actions/workflows/ci.yml/badge.svg)](https://github.com/OnFreund/pymonoprice/actions/workflows/ci.yml)

# pymonoprice
Python3 interface implementation for Monoprice 6 zone amplifier

## Notes
This is for use with [Home-Assistant](http://home-assistant.io)

## Usage
```python
from pymonoprice import get_monoprice

monoprice = get_monoprice('/dev/ttyUSB0')
# Valid zones are 11-16 for main monoprice amplifier
zone_status = monoprice.zone_status(11)

# Print zone status
print('Zone Number = {}'.format(zone_status.zone))
print('Power is {}'.format('On' if zone_status.power else 'Off'))
print('Mute is {}'.format('On' if zone_status.mute else 'Off'))
print('Public Announcement Mode is {}'.format('On' if zone_status.pa else 'Off'))
print('Do Not Disturb Mode is {}'.format('On' if zone_status.do_not_disturb else 'Off'))
print('Volume = {}'.format(zone_status.volume))
print('Treble = {}'.format(zone_status.treble))
print('Bass = {}'.format(zone_status.bass))
print('Balance = {}'.format(zone_status.balance))
print('Source = {}'.format(zone_status.source))
print('Keypad is {}'.format('connected' if zone_status.keypad else 'disconnected'))

# Turn off zone #11
monoprice.set_power(11, False)

# Mute zone #12
monoprice.set_mute(12, True)

# Set volume for zone #13
monoprice.set_volume(13, 15)

# Set source 1 for zone #14
monoprice.set_source(14, 1)

# Set treble for zone #15
monoprice.set_treble(15, 10)

# Set bass for zone #16
monoprice.set_bass(16, 7)

# Set balance for zone #11
monoprice.set_balance(11, 3)

# Restore zone #11 to its original state
monoprice.restore_zone(zone_status)
```

## Usage with asyncio

With `asyncio` flavor all methods of Monoprice object are coroutines.

```python
import asyncio
from pymonoprice import get_async_monoprice

async def main():
    monoprice = await get_async_monoprice('/dev/ttyUSB0')
    zone_status = await monoprice.zone_status(11)
    if zone_status.power:
        await monoprice.set_power(zone_status.zone, False)

asyncio.run(main())
```

## Testing PRs

Every pull request automatically publishes a test build as a GitHub pre-release. You can find the install command in the PR comment posted by the bot, or on the [Releases page](https://github.com/OnFreund/pymonoprice/releases) (pre-releases are tagged `pr-{number}`).

**pip:**
```
pip install https://github.com/OnFreund/pymonoprice/releases/download/pr-42/pymonoprice-0.0.0.dev42-py3-none-any.whl
```

**Home Assistant** — temporarily update your integration's `manifest.json` to use the PEP 508 URL form so HA doesn't overwrite it on restart:
```json
{
  "requirements": ["pymonoprice @ https://github.com/OnFreund/pymonoprice/releases/download/pr-42/pymonoprice-0.0.0.dev42-py3-none-any.whl"]
}
```
Replace `42` with the actual PR number. Revert to the pinned version (e.g. `pymonoprice==0.6`) after testing.

The install URL is stable for the lifetime of the PR — new commits to the same PR reuse the same tag and wheel name, so you don't need to update `manifest.json` if more commits are pushed.

The pre-release and comment are deleted automatically when the PR is merged or closed.
