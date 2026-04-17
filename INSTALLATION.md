# Installation Instructions - Fixed KitchenOwl Integration

## Quick Installation via HACS

1. **Remove the old integration** (if already installed):
   - Go to Settings → Devices & Services
   - Find any existing KitchenOwl entries and remove them
   - Go to HACS → Integrations
   - Find KitchenOwl and uninstall it
   - Restart Home Assistant

2. **Install the fixed version**:
   - Copy the contents of `custom_components/kitchenowl/` to your Home Assistant's `custom_components/kitchenowl/` directory
   - Or, if using HACS with a custom repository, point to your fixed repository

3. **Restart Home Assistant**:
   - Go to Settings → System → Restart
   - Wait for Home Assistant to fully restart

4. **Add the integration**:
   - Go to Settings → Devices & Services
   - Click "Add Integration"
   - Search for "KitchenOwl"
   - The config flow should now load successfully (no 500 error!)
   - Enter your KitchenOwl instance details:
     - Host URL (e.g., https://your-kitchenowl-instance.com)
     - Access Token
     - SSL verification preference
   - Select your household
   - Complete the setup

## Manual Installation

1. **Locate your Home Assistant configuration directory**:
   - This is typically `/config/` in Home Assistant OS/Supervised
   - Or `~/.homeassistant/` in Home Assistant Core

2. **Copy the fixed files**:
   ```bash
   cd /path/to/your/homeassistant/config
   mkdir -p custom_components/kitchenowl
   # Copy all files from custom_components/kitchenowl/ to this directory
   ```

3. **Restart and configure** (follow steps 3-4 above)

## File Structure

Your `custom_components/kitchenowl/` directory should contain:
- `__init__.py` (FIXED)
- `config_flow.py` (FIXED)
- `coordinator.py`
- `const.py`
- `todo.py`
- `manifest.json`
- `strings.json`
- `icons.json`
- `translations/en.json`
- `README.md`

## What Was Fixed?

See `FIX_REPORT.md` for a detailed explanation of the issues and fixes.

**Summary:**
1. Changed type alias definition from `type` statement to `TypeAlias` annotation
2. Fixed VERSION from 0 to 1 in config flow
3. Used TYPE_CHECKING conditional import to avoid circular import issues

## Troubleshooting

### If you still see the 500 error:

1. **Check logs**:
   - Go to Settings → System → Logs
   - Look for any error messages related to KitchenOwl
   - Share the full traceback if asking for help

2. **Verify Python version**:
   - Home Assistant 2024.4+ requires Python 3.12+
   - Check: Settings → System → Repairs

3. **Clear cache**:
   - Delete `custom_components/kitchenowl/__pycache__/` directory
   - Restart Home Assistant

4. **Check dependencies**:
   - The integration requires `kitchenowl-python` package
   - This should be installed automatically
   - If not, check Home Assistant logs for pip installation errors

### If KitchenOwl connection fails:

1. **Verify your KitchenOwl instance is accessible**
2. **Check your access token is valid**
3. **Ensure SSL settings match your instance** (enable/disable SSL verification as needed)
4. **Check firewall rules** if Home Assistant can't reach your instance

## Support

If you encounter issues:
1. Check the Home Assistant logs for detailed error messages
2. Verify all files were copied correctly
3. Ensure you're running Home Assistant 2024.4 or later
4. Check that your KitchenOwl instance is accessible

## Credits

- Original integration by @TomBursch
- Fixes based on Home Assistant 2024-2026 best practices and API changes
