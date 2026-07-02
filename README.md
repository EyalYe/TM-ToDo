# TM-ToDo — two task apps sharing one library (TaskMaster-C3)

An example that shows how to ship **two apps that share a library** and **select one to
build**, for a [TaskMaster-C3](https://github.com/EyalYe/TaskMaster) device. It contains
two task-list apps:

- **Cloud** (`apps/app_cloud`) — tasks straight from **Todoist** over HTTPS
- **Local** (`apps/app_local`) — tasks from a **LAN task server**

Both are just a thin network layer over the **same** shared library
[`components/todo_common/tasks.h`](components/todo_common/tasks.h) — the task list view,
offline cache, and write-replay queue live there once, not copied per app. Each app
`REQUIRES todo_common`, so there's a single source of truth.

## Pick one

They're **alternative task sources**, so you build **one** at a time — choose it in
[`apps.yaml`](apps.yaml) (uncomment one, comment the other):

```yaml
apps:
  - name: app_cloud            # Todoist
    path: apps/app_cloud
  # - name: app_local          # LAN server
  #   path: apps/app_local
```

That's the whole "select one app" mechanism — the shared library builds either way; the
manifest decides which app ships. (Nothing stops you from listing both, but two task
lists on one device is rarely what you want.)

## Configure

Set the source in the **setup form** (or **Settings → Web config**): Cloud wants a
Todoist API token; Local wants your server URL + token. Until configured, the app hides
itself from the Launcher.

## Build, flash, update

Same as any TaskMaster-C3 app — no core to compile, nothing hosted:

- **CI / local build** produces two files: **`firmware-merged.bin`** (first USB flash) and
  **`taskmaster_c3_app.bin`** (OTA). Use the right one:
- **First flash (USB):** `python tools/flash.py --bin firmware-merged.bin` (or bare
  `python tools/flash.py` for a local build).
- **Update over your LAN (OTA):** `python tools/ota_serve.py --bin taskmaster_c3_app.bin`
  (or bare for a local build) → point the device `fw_url` at the URL it prints.

Full app contract: [`docs/APP_API.md`](docs/APP_API.md). Agent/contributor context:
[`docs/agents/`](docs/agents/).
