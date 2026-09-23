# Fix Windows/Linux Dual-Boot Clock

## Problem

When dual-booting **Windows and Linux**:

- **Windows → Linux:** Time is correct.
- **Linux → Windows:** Windows time is shifted because the hardware clock (RTC) was written in UTC.

This happens because:

- **Linux** uses **UTC** for the hardware clock by default.
- **Windows** treats the hardware clock as **local time** by default.

## Recommended Fix: Make Windows Use UTC

Run **Command Prompt or PowerShell as Administrator**:

```powershell
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /t REG_DWORD /d 1 /f
```

Then reboot Windows.

> [!NOTE]
>
> Windows does not provide a normal Settings option for this; `RealTimeIsUniversal` must be configured through the Registry.

## Alternative: Make Linux Use Local Time

Not recommended, but possible:

```bash
sudo timedatectl set-local-rtc 1 --adjust-system-clock
```

It is generally preferable to **keep Linux/RTC in UTC and configure Windows to use UTC**.
