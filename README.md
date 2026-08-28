# DXLog Second Operator

A DXLog.net custom form for a **second operator on the same radio and the same live DXLog log**.

The project is designed for SO2V-style operation where the main DXLog operator uses **VFO A** and a second operator uses **VFO B**, while sharing DXLog's contest log and serial-number server.

Current version: **v0.1.47**

[Download the v0.1.47 source package](DXLogSecondOperator-v0.1.47.zip)

> This is a third-party DXLog.net custom form and is not an official DXLog.net component.

## Features

- Separate **Second Operator** logging window using the same live DXLog log.
- Radio 1 / **VFO B** frequency and mode tracking.
- DXLog-native dupe checking and worked-band display.
- Super Check Partial using `MASTER.SCP`.
- DXLog **Serial Number Server** reservation — no local `last + 1` counter.
- Compact DXLog-style 10-QSO history and live entry line.
- Callsign / received-serial workflow using the keyboard only.
- Previous-QSO editing with cursor keys.
- True physical-keyboard isolation using the **Interception** driver:
  - Keyboard A -> normal DXLog.
  - Keyboard B -> Second Operator only.
- Stable keyboard selection by hardware ID, so device-number changes after reboot do not matter.
- Optional dual footswitch PTT:
  - **PTT A** -> split off, TX on VFO A.
  - **PTT B** -> split on, TX on VFO B.
- Configurable CTS / DSR / CD footswitch inputs.
- Configurable UI colours.
- Input helper runs silently when launched automatically.

## Requirements

- Windows 10/11.
- DXLog.net **2.6.32** or compatible.
- Visual Studio 2022 with **.NET Framework 4.8** development tools.
- Build target: **x86**.
- DXLog installed in the normal location:

```text
C:\Program Files (x86)\DXLog.net\
```

The plugin project references:

```text
DXLog.net.exe
DXLogDAL.dll
IOComm.dll
```

For a physically isolated second keyboard you also need the Interception keyboard/mouse driver and the **x86** `interception.dll`.

## Build

Extract `DXLogSecondOperator-v0.1.47.zip`, then open:

```text
DXLogSecondOperator.sln
```

in Visual Studio 2022 and select:

```text
Debug | x86
```

or:

```text
Release | x86
```

Build the solution. It produces:

```text
DXLogSecondOperator\bin\x86\Debug\DXLogSecondOperator.dll
SecondOperatorInput\bin\x86\Debug\SecondOperatorInput.exe
```

The Release paths are identical except for `Release` instead of `Debug`.

You can also run:

```bat
build-release.bat
```

from a Visual Studio Developer Command Prompt.

## Install

The easiest method is to build first and then run:

```bat
install.bat
```

This copies the plugin to:

```text
%APPDATA%\DXLog.net\CustomForms\DXLogSecondOperator.dll
```

and the keyboard helper to:

```text
%APPDATA%\DXLog.net\SecondOperator\Input\SecondOperatorInput.exe
```

Restart DXLog after installing or replacing the DLL.

### Interception keyboard isolation

Interception is **not bundled** with this repository.

1. Download the Interception project/release (`oblitum/Interception`).
2. Open an **Administrator Command Prompt** in its `command line installer` folder.
3. Run:

```bat
install-interception.exe /install
```

4. Reboot Windows.
5. Copy the **x86** DLL:

```text
Interception\library\x86\interception.dll
```

beside the helper:

```text
%APPDATA%\DXLog.net\SecondOperator\Input\SecondOperatorInput.exe
%APPDATA%\DXLog.net\SecondOperator\Input\interception.dll
```

Do **not** use the x64 DLL: `SecondOperatorInput.exe` is an x86 application.

## DXLog Serial Number Server

The Second Operator deliberately uses DXLog's native number server. Configure DXLog before operating:

1. Enable DXLog networking.
2. **Configure network -> Serial number server**: enabled.
3. **Options -> Networking -> Use serial number server**: enabled.
4. **Options -> Networking -> Spacebar reserves serial number**: enabled for the normal DXLog operator.
5. Enabling **Block logging if serial reservation unsuccessful** is recommended.

The Second Operator never invents a serial locally. It calls DXLog's `ReserveQSONumber(...)` API.

## Select the Second Operator keyboard

Open **Setup** in the Second Operator window.

The keyboard selector uses the physical keyboard's hardware ID, similar to DXLog's own keyboard selector. You can either select a listed keyboard or click **Identify...** and press one key on the keyboard you want to dedicate to Operator 2.

For diagnostics you can also run:

```bat
SecondOperatorInput.exe --identify
```

or:

```bat
list-keyboards.bat
```

When isolation is working, the selected keyboard is consumed by the helper and sent only to the Second Operator plugin. Other keyboards continue to Windows/DXLog normally.

## Operating workflow

Typical new-QSO workflow:

1. Type the callsign on **Keyboard B**.
2. Press **Space** to reserve the next DXLog serial and move to the received serial field.
3. Enter the received serial. RST defaults to `59` on SSB.
4. Press **Space** to toggle back to the callsign if you need to correct it; Space toggles between Callsign and received Serial without logging.
5. Press **Enter** to log the QSO.
6. The entry clears and returns to Callsign.
7. **Alt-W** abandons/clears the current entry and returns to Callsign.

A number already reserved by DXLog remains consumed if an entry is abandoned; this avoids reusing an authoritative number-server reservation.

## Edit a previous QSO

From an empty live entry:

- **Up** -> load the previous QSO.
- Further **Up/Down** -> move through logged QSOs.
- **Left/Right** -> move between editable fields.
- **Space** -> toggle Callsign / received Serial.
- **Enter** -> save the edit back to the existing DXLog QSO.
- **Alt-W** or **Esc** -> cancel the edit and return to the live entry.

## Callsign colours / status

The console uses DXLog/current-log information plus `MASTER.SCP`:

- **Red**: dupe according to the current contest/band/mode rules.
- **Green**: valid/known call.
- **Yellow**: syntactically valid but not found in SCP/current log.
- The status below the entry line shows states such as **VALID CALL**, **DUPE**, or **INCOMPLETE / INVALID CALL**.

## Footswitch PTT

The Setup dialog supports two independent serial-port status inputs. Each can use **CTS**, **DSR**, or **CD**, with optional polarity inversion.

- **PTT A**: Radio 1, split OFF, transmit on VFO A.
- **PTT B**: Radio 1, split ON, transmit on VFO B.
- On release, the plugin de-keys and restores the previous VFO/split state.
- If DXLog reports the radio is already transmitting, Operator-2 PTT is ignored rather than taking ownership of an existing transmission.

The two pedals may use different COM ports or different status lines on one adapter.

PTT notes are included in the source package under `docs/PTT.md`.

## Files in the source package

```text
DXLogSecondOperator/     DXLog custom-form plugin
SecondOperatorInput/    Interception keyboard/mouse isolation helper
install.bat             installs a built plugin/helper into AppData
build-release.bat       builds Release | x86
list-keyboards.bat      lists Interception keyboard IDs
run-input-identify.bat  interactive keyboard/mouse identification
```

## Notes

- Do not let the helper open the radio CAT COM port. DXLog owns CAT; the plugin uses DXLog's radio abstraction.
- The project currently targets Radio 1 / VFO B for the Second Operator.
- Interception is a separate third-party dependency and is intentionally not redistributed here.
- `MASTER.SCP` must be available to DXLog for Super Check Partial data.

## Version

Current source: **v0.1.47**.

See [CHANGELOG.md](CHANGELOG.md) for recent development history.
