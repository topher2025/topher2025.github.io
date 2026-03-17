---
title: "How I Implemented Remote Firmware Updates on a Microcontroller"
date: 2026-03-17
categories:
  - Technical
tags:
  - Python
  - Firmware
---

One of my interests is integrating low-level programming and firmware with higher-level backend systems. This led me to explore how to remotely update firmware on a microcontroller over a network.

## Setting Up:

To start, I set up a simple client-server architecture. The server would host the firmware updates, and the microcontroller (client) would periodically check for updates and apply them. I used a simple Flask API as my backend, with a Pi Pico W running micoPython as the client.

```python
import urequests

def gather_updates():
    try:
        resp = urequests.get("http://myserver.com/api/updates")
        need_update = resp.json().get("update_available", False)
        resp.close()
        return need_update
    except Exception as e:
        print("Error checking for updates:", e)
        return False
    
```

## Starting Point: Sub-Processes

The first approach to come to mind was to make a new file called `update-XX.py` for every update, and then dynamically load and execute it.
```python
import urequests
def run_update(update_num):
    filename = f"update_{update_num}.py"

    try:
        with open(filename) as f:
            code = f.read()

        namespace = {}
        exec(code, namespace)
        
        return True

    except OSError:
        print("Update file not found:", filename)
    except Exception as e:
        print("Error running update:", e)
        
    return False
```

This works, but only for a few iterations. After a while, the number of files becomes unmanageable, and the overhead of starting a new process for each update becomes significant.

## Re-Write `main.py` to Handle Updates

Instead of creating a new file for each update, I set it up to write over `main.py` and reboot.

```python
import urequests
import os
import machine

def run_update(update_num):

    try:
        resp = urequests.get(f"http://myserver.com/api/updates/update")
        code = resp.text
        resp.close()
        
        with open("update.py", "w") as f:
            f.write(code)
        
            
        os.rename("update.py", "main.py")
        machine.reset()
        return True

    except Exception as e:
        print("Error running update:", e)
```

This is dramatically cleaner and more efficient. It avoids the overhead of multiple files and processes, and allows for seamless updates. The only downside of this is it requires a machine reboot for each update.


## Taking It Further: Backups

The next step would be to implement a backup system, where the current `main.py` is saved to `main_backup.py` before being overwritten, and can be restored if the update fails. This would add robustness to the system and allow for safer updates. This could be done with a `.flag` file that indicates whether this is the first boot with the new update, and if it fails or times out, the backup can be restored.

## What I Took Away

- MicroPython doesn’t support true subprocesses, so dynamic execution is the closest alternative—but not scalable
- Rewriting the main script is a simpler and more maintainable update strategy



I'm planning to explore update validation next, where the server can send a hash of the update code, and the client can verify it before applying the update. This would add an extra layer of security to ensure that only authorized updates are applied.
