# Redmi Note 11 MAX77729 Temperature-Fault Workaround

An experimental Linux kernel patch for Redmi Note 11 (`spes`/`spesn`) devices using the MAX77729 fuel gauge and charger.

## Problem

After a battery replacement, the fuel gauge may report a raw battery temperature of `-295` (−29.5 °C), or fail to return a temperature reading. The reported value may indicate faulty temperature telemetry rather than the battery’s actual temperature.

## Workaround

The patch detects the known raw value and temperature-read failures. When temperature telemetry is invalid or unavailable, it:

- Disables fast charging.
- Limits charging current to 500 mA.
- Stops charging at 80% raw state of charge and allows it to resume at or below 78%.
- Disables charging if battery-management telemetry is unavailable.

When the temperature readings appear valid again, the fault-specific limits are removed. Normal device thermal protections should remain in place.

## Status and safety

**This patch has not been tested on a phone. It is experimental and is not intended for daily use.**

The changes compiled in the prior kernel build, but runtime behavior and safety remain unverified. The workaround does not repair the fuel gauge, establish why the readings are wrong, or prove that the battery is safe. Do not flash it without confirming compatibility with your exact device and boot/vendor_boot images, and having a recovery method available.

## Patch

The repository contains a focused patch for the MAX77729 fuel-gauge and charger drivers. Apply it from the root of a compatible kernel source tree:

```sh
git apply max77729-temperature-fault-workaround.patch
