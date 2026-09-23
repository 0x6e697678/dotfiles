# Sync Bluetooth LE Connection Between Two OSes

When dual-booting Linux and Windows, Bluetooth devices may need to be paired again every time you switch between them. To make the same device connect seamlessly without re-pairing:

1. Pair and connect on Linux.

2. Pair and connect on Windows.

3. Shut down the Bluetooth device you want to sync.

4. Boot to Linux.

5. Mount the Windows partition.

6. Go to:

   ```text
   <windows-mount>/Windows/System32/config
   ```

7. Install `chntpw`:

   ```bash
   sudo pacman -S chntpw
   ```

   Then open the `SYSTEM` registry hive:

   ```bash
   chntpw -e SYSTEM
   ```

8. Go to the key:

   ```text
   ControlSet001\Services\BTHPORT\Parameters\Keys\<computer-bluetooth-mac>\<device-bluetooth-mac>
   ```

   **Note:** It can be `Parameters`, `Parameters1`, or anything similar. Use `ls` to list subkeys under `BTHPORT`. Use `?` to see available commands in `chntpw`.

9. Rename the device's Bluetooth MAC directory under `/var/lib/bluetooth/` to the one found in the Windows registry.

   Usually, only one byte of the MAC address changes when pairing is done.

10. Open the device's `info` file and replace the following values:

    | Windows Registry | Linux `info` file                                 |
    | ---------------- | ------------------------------------------------- |
    | `IRK`            | `Key` in `IdentityResolvingKey`                   |
    | `CSRK`           | `Key` in `LocalSignatureKey                       |
    | `LTK`            | `Key` in `LongTermKey` or `PeripheralLongTermKey` |
    | `ERand`          | `Rand` in `LongTermKey`                           |
    | `EDIV`           | `Ediv` in `LongTermKey`                           |

    For `IRK`, `CSRK`, and `LTK`, use `hex` in `chntpw` to display the raw value. For example:

    ```text
    hex IRK
    ```

    The output may look like:

    ```text
    Value <IRK> of type REG_BINARY (3), data length 16 [0x10]
    :00000  XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX Q.L.....9.|Ty!..
    ```

    Keep only the hexadecimal bytes:

    ```text
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    ```

    Remove the spaces and use the resulting value as `Key` in the corresponding section of the Linux `info` file.

    Do the same for `CSRK` and `LTK`.

    **Note:** On some devices, `CSRK` may be missing. If so, just skip it.

    **Converting `ERand`**:

    - Display it with:

      ```text
      hex ERand
      ```

    - Suppose the value is:

      ```text
      AB CD EF
      ```

    - Reverse the byte order:

      ```text
      EF CD AB
      ```

    - Convert the resulting hexadecimal number to decimal and use it as `Rand`.

11. Restart the Bluetooth service:

    ```bash
    sudo systemctl restart bluetooth
    ```

12. Power on the Bluetooth device.
