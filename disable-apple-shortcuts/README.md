## Disable Default Apple Hotkeys Module for nix-darwin

This Nix module allows you to declaratively disable a predefined set of default Apple keyboard shortcuts (Symbolic Hotkeys).

### How to Use

1.  **Save the Module**: Place the `disable-apple-default-hotkeys.nix` file into your nix-darwin configuration directory (e.g., alongside your `darwin-configuration.nix` or in a `./modules` subdirectory).

2.  **Import and Use in your nix-darwin configuration**:

    Add the relevant bits from the following to your `darwin-configuration.nix` (or relevant Nix file where you manage system defaults).

    ```nix
    # In your darwin-configuration.nix or related file
    { lib, config, ... }: # Ensure 'lib' is available if not already

    let
      # Import your hotkey disabling module.
      # Adjust the path if necessary, e.g., ./modules/disable-apple-default-hotkeys.nix
      disabledHotkeysSettings = import ./disable-apple-default-hotkeys.nix { inherit lib; };
    in
    {
      # ... other nix-darwin configurations

      # Note: this is to avoid a logout/login to make the preferences changes take effect
      system.activationScripts.postActivation.text = ''
    sudo -u YOUR_USERNAME /System/Library/PrivateFrameworks/SystemAdministration.framework/Resources/activateSettings -u
    '';

      system.defaults.CustomUserPreferences = {
        # Your other custom preferences
        # e.g "com.some.other.domain" = { someSetting = true; };
      } // disabledHotkeysSettings; # Merge the hotkey disabling settings

    }
    ```

3.  **Rebuild your System**:
    Apply the configuration using your nix-darwin rebuild command:

    ```bash
    sudo darwin-rebuild switch
    ```

4.  **Apply Changes**:
    If for whatever reason you still see the old shortcuts in 'System Settings', Try logging out and then logging back into your macOS user account.

## This will configure your system to disable the Apple Symbolic Hotkeys specified in the `editMe_disableHotKeys` list within the module.
