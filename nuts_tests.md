# How To

## Remove credentials from inventory

NUTS use Nornir for inventory, remove `username` and `password` from hosts or groups and use Nornir's inventory transform function while exporting credentials as environment variables.

```shell
export NORNIR_INVENTORY_TRANSFORM_FUNCTION=nornir_utils.plugins.inventory.load_credentials
export NORNIR_USERNAME='your-username-here'
export NORNIR_PASSWORD='your-secret-here'
```
