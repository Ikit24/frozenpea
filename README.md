# Frozenpea

A productivity app which locks the user out from the machine after a set amount of minutes to prevent RSI-related problems and support a healthier work-life.

## Features

- Notification sound upon startup and before the break starts
- Automatic screen lock after the set amount of minutes
- Background window display during rest periods
- Customizable notifications on startup
  - **Note:** Samples have a slight delay after playing. Avoid clicking everywhere, because the system still registers and carries out those commands too.
- Delay the break once from the "Break in 1 minute" warning window
- Helps prevent repetitive strain injuries (RSI)
- Promotes healthier work habits

## System Requirements

- Linux with an X11 display server (tested on Pop!_OS with GNOME, via XWayland)
- Go 1.21 or higher

## Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/Ikit24/frozenpea.git
   cd frozenpea
   ```

2. **Install dependencies**:
   ```bash
   go mod download
   ```

3. **Run the application**:
   ```bash
   go run .
   ```

Go 1.21+ can be downloaded from https://go.dev/dl/

## Usage

On startup you can set the work and break duration.
The "Cancel session" button stops the program completely.
Once the settings are confirmed, the application starts and you will be notified one minute before the break.
When the break warning appears, you can enter a number of minutes and press "Confirm break delay" to postpone the break once.
After the break starts you cannot interact with the OS.

## Automatic Startup Configuration (Linux)

### 1. Compile the program

```bash
cd ~/projects/code/frozenpea
go build -o frozenpea
```

### 2. Create the startup script

Create `~/startup.sh` (replace `ati` with your username):

```bash
#!/bin/sh
sleep 10
cd /home/ati/projects/code/frozenpea || exit 1
exec ./frozenpea >> /home/ati/frozenpea.log 2>&1
```

Make it executable:

```bash
chmod +x ~/startup.sh
```

The `cd` is required because the app loads its images and sounds from `./assets`.

The `sleep 10` gives the desktop session time to finish loading before the app opens its windows. The startup window appears about 10 seconds after you log in.

### 3. Create the autostart entry

```bash
mkdir -p ~/.config/autostart
nano ~/.config/autostart/frozenpea.desktop
```

```ini
[Desktop Entry]
Type=Application
Name=FrozenPea Break Timer
Exec=/home/ati/startup.sh
X-GNOME-Autostart-enabled=true
```

### Testing

Test the script without rebooting:

```bash
~/startup.sh
```

Check the log file for any errors:

```bash
cat ~/frozenpea.log
```

### Managing the autostart

**Stop the program:**

```bash
pkill frozenpea
```

**Remove from startup:**

```bash
rm ~/.config/autostart/frozenpea.desktop
```

> After every code change, run `go build -o frozenpea` again.

## License

MIT License

Copyright (c) 2026 Attila Szasz

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
