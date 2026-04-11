# HyprPanel Wallbash Template

![241005_14h24m09s_screenshot](https://github.com/user-attachments/assets/355aa7f0-856b-47f6-8ced-58bc0c4a3481)
![241005_14h26m11s_screenshot](https://github.com/user-attachments/assets/e7551bec-573c-4d37-91b9-de9400176cac)
![241005_14h19m51s_screenshot](https://github.com/user-attachments/assets/11f40837-08fe-4979-b16e-b1d0a6fd4fcd)

## Wallbash Template for HyprPanel

This template is designed for use with the latest version of [HyprPanel](https://hyprpanel.com/) (Astal-based). 

> **NOTE:** This is not a standalone package. Ensure you have HyDE installed and a working HyprPanel setup.
> **COMPATIBILITY:** This template has been updated to support the new `astal` backend of HyprPanel. If you are using an older version of HyprPanel based on `ags`, you might need to use an older version of this repository.

## Available Templates (Default vs Split)

Because of how HyprPanel handles button styling, this repository provides two different `.dcol` files. You must choose **only one** based on your HyprPanel settings:

*   **`hyprpanel.dcol`**: Designed for the regular/default bar style.
    <img width="2558" height="108" alt="image" src="https://github.com/user-attachments/assets/bcd2ec20-f732-4edf-9ade-412254aee90f" />

*   **`hyprpanel_split.dcol`**: Specifically adapted for the split button style (where the icon sits on top of a background).
    <img width="2558" height="119" alt="image" src="https://github.com/user-attachments/assets/21b5d385-9b55-449b-86df-13042bc84e22" />
        
    *   *To use this template, you must enable it in HyprPanel:* Go to **Settings -> Bar -> Button Style -> change from `default` to `split`**.
         <img width="873" height="338" alt="image" src="https://github.com/user-attachments/assets/9aeb3835-9810-40f7-a824-b12d59a2fa76" />
    

## Usage

Download your desired template (`hyprpanel.dcol` **OR** `hyprpanel_split.dcol`) and place it in either `~/.config/hyde/wallbash/always/` or `~/.config/hyde/wallbash/theme/`.

### Directory Differences

According to the HyDE documentation, the directory you choose changes when the template is executed:

- **`always/`**: Templates placed here are executed during a **Theme switch**, **Wallpaper switch**, or **Mode switch**.
- **`theme/`**: Templates placed here are executed **only** during a **Theme switch** or **Mode switch**.

### Using This Template for Themes

1. **Header Line**:
    The `.dcol` file must start with the following target and command (header line) to properly trigger the Astal theme update:
    ```sh
    ${cacheDir}/landing/wallbash-hyprpanel.json | astal -i hyprpanel useTheme ${cacheDir}/landing/wallbash-hyprpanel.json
    ```
    This generates the `wallbash-hyprpanel.json` file in your cache and immediately pipes the command to Astal to apply the theme in HyprPanel.

2. **Manual/Optional Command**:
    If you need to trigger the theme reload manually via terminal, you can run:
    ```sh
    astal -i hyprpanel useTheme ~/.cache/hyde/landing/wallbash-hyprpanel.json
    ```

## Matugen Compatibility

By default, this template is fully compatible as long as you **do not use Matugen**. 

If Matugen is enabled, it forces its own color overrides onto the bar's theme, which conflicts with this Wallbash template. If you still want to use both together, you must set up a custom workaround to trigger Matugen correctly:

1. **Create a script** (e.g., `hyprpanel_matugen.sh`) in your `~/.config/hyde/wallbash/scripts/` directory. This script must automatically inject the new wallpaper path into HyprPanel's `config.json` (specifically editing the `"wallpaper.image": "/path/to/your/image"` line).
2. **Create a trigger `.dcol` file** in your `~/.config/hyde/wallbash/always/` directory. This file acts as a dummy to call your script every time the wallpaper changes. The file should look like this:
    ```sh
    /tmp/hyprpanel_trigger.txt|bash $WALLBASH_SCRIPTS/hyprpanel_matugen.sh
    # Dummy file to trigger Matugen config update on wallpaper change
    ```
3. By dynamically changing this image path with the newly applied Wallbash wallpaper, you force Matugen to trigger and overwrite the colors correctly, allowing both systems to work side by side.

<details>
<summary><b>💡 Click here for an example of the Bash script</b></summary>

You can use `jq` to safely edit the `config.json` file, and `readlink` to get the current wallpaper applied by HyDE.

**`~/.config/hyde/wallbash/scripts/hyprpanel_matugen.sh`**:
```bash
#!/bin/bash

# Path to your HyprPanel config
CONFIG_FILE="$HOME/.config/hyprpanel/config.json"

# Get the current wallpaper set by HyDE/Wallbash
CURRENT_WALL=$(readlink -f ~/.cache/hyde/wall.set)

# Use jq to update the wallpaper.image line safely
if command -v jq &> /dev/null && [ -f "$CONFIG_FILE" ]; then
    jq --arg wp "$CURRENT_WALL" '."wallpaper.image" = $wp' "$CONFIG_FILE" > "${CONFIG_FILE}.tmp" && mv "${CONFIG_FILE}.tmp" "$CONFIG_FILE"
else
    echo "Error: jq is not installed or config.json not found."
fi
```
*(Make sure to make the script executable with `chmod +x ~/.config/hyde/wallbash/scripts/hyprpanel_matugen.sh`)*

</details>

## TODO
- Dedicated installation

## Contributions

- Enhance the template by adding more contrast to some elements.
- Avoid making breaking changes.
- Feel free to open a PR for color corrections!

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/A)