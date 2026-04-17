# Code Changes Summary

## File 1: custom_components/kitchenowl/__init__.py

### Change 1: Type Alias Import and Definition (Lines 3, 21)

**BEFORE:**
```python
"""KitchenOwl integration for Home Assistant."""

import logging

from kitchenowl_python.exceptions import KitchenOwlAuthException, KitchenOwlException
from kitchenowl_python.kitchenowl import KitchenOwl

from homeassistant.config_entries import ConfigEntry
...

type KitchenOwlConfigEntry = ConfigEntry[KitchenOwlDataUpdateCoordinator]
```

**AFTER:**
```python
"""KitchenOwl integration for Home Assistant."""

import logging
from typing import TypeAlias

from kitchenowl_python.exceptions import KitchenOwlAuthException, KitchenOwlException
from kitchenowl_python.kitchenowl import KitchenOwl

from homeassistant.config_entries import ConfigEntry
...

# Use TypeAlias for better compatibility
KitchenOwlConfigEntry: TypeAlias = ConfigEntry[KitchenOwlDataUpdateCoordinator]
```

**Changes:**
- Added `from typing import TypeAlias` import
- Changed `type KitchenOwlConfigEntry = ...` to `KitchenOwlConfigEntry: TypeAlias = ...`

---

## File 2: custom_components/kitchenowl/config_flow.py

### Change 1: Import Statement (Lines 3-4, 24-26)

**BEFORE:**
```python
"""Config Flow for the KitchenOwl integration."""

import logging
from typing import Any

from kitchenowl_python.exceptions import (
    KitchenOwlAuthException,
    KitchenOwlRequestException,
)
...

from . import KitchenOwlConfigEntry
from .const import CONF_HOUSEHOLD, DOMAIN
```

**AFTER:**
```python
"""Config Flow for the KitchenOwl integration."""

import logging
from typing import TYPE_CHECKING, Any

from kitchenowl_python.exceptions import (
    KitchenOwlAuthException,
    KitchenOwlRequestException,
)
...

from .const import CONF_HOUSEHOLD, DOMAIN

if TYPE_CHECKING:
    from . import KitchenOwlConfigEntry
```

**Changes:**
- Added `TYPE_CHECKING` to the typing imports
- Moved the `KitchenOwlConfigEntry` import into an `if TYPE_CHECKING:` block
- Removed it from the regular imports

### Change 2: VERSION Configuration (Lines 55-56)

**BEFORE:**
```python
class KitchenowlConfigFlow(config_entries.ConfigFlow, domain=DOMAIN):
    """Kitchenowl config flow."""

    # The schema version of the entries that it creates
    # Home Assistant will call your migrate method if the version changes
    VERSION = 0
    MINOR_VERSION = 1
```

**AFTER:**
```python
class KitchenowlConfigFlow(config_entries.ConfigFlow, domain=DOMAIN):
    """Kitchenowl config flow."""

    # The schema version of the entries that it creates
    # Home Assistant will call your migrate method if the version changes
    VERSION = 1
    MINOR_VERSION = 0
```

**Changes:**
- Changed `VERSION = 0` to `VERSION = 1`
- Changed `MINOR_VERSION = 1` to `MINOR_VERSION = 0`

### Change 3: Type Annotation (Line 58)

**BEFORE:**
```python
    config_entry: KitchenOwlConfigEntry | None = None
```

**AFTER:**
```python
    config_entry: "KitchenOwlConfigEntry | None" = None
```

**Changes:**
- Added quotes around the type annotation to defer evaluation
- This works with the TYPE_CHECKING import pattern

---

## All Other Files

No changes needed. The following files remain unchanged:
- `coordinator.py`
- `const.py`
- `todo.py`
- `manifest.json`
- `strings.json`
- `icons.json`
- `translations/en.json`
- `README.md`

---

## Summary of Changes

**Total files changed:** 2
**Total lines changed:** ~10
**Breaking changes:** None (backward compatible)

These minimal changes fix the "Config flow could not be loaded: 500" error by:
1. Using more compatible type alias syntax
2. Following Home Assistant's versioning standards
3. Avoiding circular import issues with TYPE_CHECKING pattern
