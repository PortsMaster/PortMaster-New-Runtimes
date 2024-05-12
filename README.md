# PortMaster New Runtimes Playground

Goals are:
- easy to use
- easy to have different architectures of a runtime
- more than likely will be a zip like a port.
- reduce the crud required to run them

###  Old godot 3.5.2 port looks like:
```sh
runtime="frt_3.5.2"
if [ ! -f "$controlfolder/libs/${runtime}.squashfs" ]; then
  # Check for runtime if not downloaded via PM
  if [ ! -f "$controlfolder/harbourmaster" ]; then
    echo "This port requires the latest PortMaster to run, please go to https://portmaster.games/ for more info." > /dev/tty0
    sleep 5
    exit 1
  fi

  $ESUDO $controlfolder/harbourmaster --quiet --no-check runtime_check "${runtime}.squashfs"
fi

# Setup Godot
godot_dir="$HOME/godot"
godot_file="$controlfolder/libs/${runtime}.squashfs"
$ESUDO mkdir -p "$godot_dir"
$ESUDO umount "$godot_file" || true
$ESUDO mount "$godot_file" "$godot_dir"
PATH="$godot_dir:$PATH"

export FRT_NO_EXIT_SHORTCUTS=FRT_NO_EXIT_SHORTCUTS

$ESUDO chmod 666 /dev/uinput
$GPTOKEYB "$runtime" -c "./bananaguy2.gptk" &
SDL_GAMECONTROLLERCONFIG="$sdl_controllerconfig" "$runtime" $GODOT_OPTS --main-pack "gamedata/Bananaguy2.pck"
```

### New godot 3.5.2 will look like:
```sh
runtime="godot_3.5.2"

if [ ! -f "$controlfolder/runtimes/${runtime}.txt" ]; then
  if [ ! -f "$controlfolder/harbourmaster" ]; then
    echo "This port requires the latest PortMaster to run, please go to https://portmaster.games/ for more info." > /dev/tty0
    sleep 5
    exit 1
  fi

  $ESUDO $controlfolder/harbourmaster --quiet --no-check runtime_check "$runtime"
fi

source "$controlfolder/runtimes/${runtime}.txt"

$GPTOKEYB $GODOT_BINARY -c "blah.gptk" &
$GODOT_RUN "gamedata/Bananaguy2.pck"
```

# runtime.json

The runtime is defined by a json file that looks like the following:
```json
{
    "version": 1,
    "name": "godot_3.5.2",
    "files": [
        "godot_3.5.2.txt"
    ],
    "build": null,
    "builds": [
        {
            "title": "Godot/FRT 3.5.2",
            "tag": "frt_arch64",
            "reqs": ["aarch64", "!x11"],
            "files": [
                "godot_3.5.2/",
                "godot_3.5.2/aarch64.txt",
                "godot_3.5.2/frt_aarch64_3.5.2"
            ]
        },
        {
            "title": "Godot/FRT 3.5.2",
            "tag": "frt_armhf",
            "reqs": ["armhf"],
            "files": [
                "godot_3.5.2/",
                "godot_3.5.2/armhf.txt",
                "godot_3.5.2/frt_armhf_3.5.2"
            ]
        },
        {
            "title": "Godot/X11 3.5.2",
            "tag": "x11_arch64",
            "reqs": ["aarch64", "x11"],
            "files": [
                "godot_3.5.2/",
                "godot_3.5.2/aarch64.txt",
                "godot_3.5.2/x11_aarch64_3.5.2"
            ]
        },
        {
            "title": "Godot/X11 3.5.2",
            "tag": "x11_x86_64",
            "reqs": ["x64_64"],
            "files": [
                "godot_3.5.2/",
                "godot_3.5.2/x86_64.txt",
                "godot_3.5.2/x11_x86_64_3.5.2"
            ]
        }
    ]
}
```

## File structure
```
runtimes/godot_3.5.2/
├── runtime.json
└── runtimes/
    ├── godot_3.5.2/
    │   ├── aarch64.txt
    │   ├── armhf.txt
    │   ├── x86_64.txt
    │   ├── frt_aarch64_3.5.2
    │   ├── frt_armhf_3.5.2
    │   ├── x11_aarch64_3.5.2
    │   └── x11_x86_64.3.5.2
    └── godot_3.5.2.txt
```

## Output

- godot_3.5.2.frt_armhf.zip
- godot_3.5.2.frt_aarch64.zip
- godot_3.5.2.x11_aarch64.zip
- godot_3.5.2.x11_x64_64.zip

## Things left to figure out:

- Install/Uninstall (moderate)
- Build system (easy)
- Distribution (easy)
- Runtime manager (moderate)

At this point in time it is a proof of concept. Nothing is tested. :)
