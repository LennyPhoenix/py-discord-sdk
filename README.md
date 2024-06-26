# Discord Game SDK for Python

> Notice 2024/05/31
> Discord's Game SDK has been largely deprecated, and I don't have the bandwidth to maintain the project so I am choosing to archive it.
> Feel free to fork or email me if you'd like to maintain the project.

A Python wrapper around Discord's Game SDK.

**NOTE**: This is entirely experimental, and may not work as intended. Please report all bugs to the [GitHub issue tracker](https://github.com/LennyPhoenix/py-discord-sdk/issues).

**Credit to [NathaanTFM](https://github.com/NathaanTFM) for creating the [original library](https://github.com/NathaanTFM/discord-game-sdk-python).**

## Installation

- Install the module:
  - With `PIP`:
    - Stable: `python -m pip install discordsdk`
    - Latest: `python -m pip install git+https://github.com/LennyPhoenix/py-discord-sdk.git`
  - With `setup.py` (latest):
    - `git clone https://github.com/LennyPhoenix/py-discord-sdk.git`
    - `cd py-discord-sdk`
    - `python -m setup install`
- Download [Discord Game SDK (2.5.6)](https://dl-game-sdk.discordapp.net/2.5.6/discord_game_sdk.zip).
- Grab the DLL from `discord_game_sdk.zip` in the `lib` directory and put it in your project's `lib` directory.

## Documentation

If you need documentation, look at [**the official Game SDK docs**](https://discord.com/developers/docs/game-sdk/sdk-starter-guide); this was made following the official documentation.
We also have some [work-in-progress wiki docs](https://github.com/LennyPhoenix/py-discord-sdk/wiki).

## Features

- Should be working:
  - **ActivityManager**
  - **ImageManager**
  - **NetworkManager**
  - **RelationshipManager**
  - **StorageManager**
  - **UserManager**

- Should be working, but need more testing:
  - **AchievementManager** (not tested at all)
  - **ApplicationManager** (especially the functions `GetTicket` and `ValidateOrExit`)
  - **LobbyManager**
  - **OverlayManager**
  - **StoreManager** (not tested at all)
  - **VoiceManager**

## Contributing

The code needs **more comments, type hinting**. You can also implement the **missing features**, or add **more tests**. Feel free to open a **pull request**!

You can also **report issues**. Just open an issue and I will look into it!

### Todo List

- Better organisation of submodules.
- CI/CD.
- Update sdk.py to use type annotations.
- Update to Discord SDK 3.2.0.

## Examples

You can find more examples in the `examples/` directory.

### Create a Discord instance

```python
import time

import discordsdk as dsdk

app = dsdk.Discord(APPLICATION_ID, dsdk.CreateFlags.default)

# Don't forget to call run_callbacks
while 1:
    time.sleep(1/10)
    app.run_callbacks()
```

### Get current user

```python
import time

import discordsdk as dsdk

app = dsdk.Discord(APPLICATION_ID, dsdk.CreateFlags.default)

user_manager = app.get_user_manager()


def on_curr_user_update():
    user = user_manager.get_current_user()
    print(f"Current user : {user.username}#{user.discriminator}")


user_manager.on_current_user_update = on_curr_user_update

# Don't forget to call run_callbacks
while 1:
    time.sleep(1/10)
    app.run_callbacks()
```

### Set activity

```python
import time

import discordsdk as dsdk

app = dsdk.Discord(APPLICATION_ID, dsdk.CreateFlags.default)

activity_manager = app.get_activity_manager()

activity = dsdk.Activity()
activity.state = "Testing Game SDK"
activity.party.id = "my_super_party_id"
activity.party.size.current_size = 4
activity.party.size.max_size = 8
activity.secrets.join = "my_super_secret"


def callback(result):
    if result == dsdk.Result.ok:
        print("Successfully set the activity!")
    else:
        raise Exception(result)


activity_manager.update_activity(activity, callback)

# Don't forget to call run_callbacks
while 1:
    time.sleep(1/10)
    app.run_callbacks()
```
