# Superhooks (discord)

This posts the superhooks event data as an embed to a discord channel, using a [webhook](https://support.discordapp.com/hc/en-us/articles/228383668-Intro-to-Webhooks).

Run this with python3.6 or above. don't be a monster.

## Installation

```
pip install git+https://github.com/seanbreckenridge/superhooks
```

Create a webhook, and store the URL in a file (just on the first line, nothing fancy) somewhere on the system.

An `[eventlistener:x]` section must be placed in `supervisord.conf` in order for `superhooks` to do its work. See the “Events” chapter in the Supervisor manual for more information about event listeners.

Assumes `superhooks-discord` is on your `$PATH`:

```
[eventlistener:superhooks-discord]
command=superhooks-discord -f "/home/user/.discord_webhook.txt" -e BACKOFF,FATAL,EXITED,UNKNOWN # 2>/some/logfile
events=PROCESS_STATE,TICK_60
#buffer_size=25
```

I've included a rudimentary flask `test_server` which can be spun up on port `8090`, so you can see what typical responses from the API are like.

### Usage

```
Usage: superhooks-discord [options]

Options:
  -h, --help            show this help message and exit
  -f FILE, --file=FILE  File which contains discord web hook.
  -e EVENTS, --events=EVENTS
                        Supervisor event(s). Can be any, some or all of
                        ('STARTING', 'RUNNING', 'BACKOFF', 'STOPPING',
                        'FATAL', 'EXITED', 'STOPPED', 'UNKNOWN') as comma
                        separated values
```

### Notes
* All the events will be buffered for 1 min and pushed to web hooks. 
