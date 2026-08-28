# Changelog

## v0.1.47

- Removed the temporary `keys N` diagnostic counter from the operating header.
- The header now simply shows `Keyboard B` when the isolated second keyboard is connected.
- No change to keyboard isolation, logging, serial reservation, VFO-B or PTT behaviour.

## v0.1.46

- Completed-QSO serial reservation flag is cleared before saving, preventing Second Operator sent serials from remaining red in DXLog.
- Space toggles between Callsign and received Serial for quick corrections.
- Enter remains the normal log/save key.
- Alt-W clears/cancels the current entry.

## v0.1.45

- Added Space-key Callsign <-> received Serial navigation without consuming a second serial reservation.

## v0.1.44

- Fixed the tenth logged QSO being hidden beneath the live entry row.

## v0.1.43

- Compact fixed-size operating window.
- Five-line worked-band pane.
- VALID / DUPE / INCOMPLETE status moved below the live entry line.

## v0.1.39-v0.1.42

- Silent auto-start of `SecondOperatorInput.exe`.
- Compact UI and title/icon cleanup.
- Main-DXLog QSO refresh and newest-QSO visibility improvements.

## v0.1.34-v0.1.38

- Stable keyboard selection by hardware ID.
- Numeric keypad handling for the isolated keyboard.
- QSO edit/navigation improvements.
- Received-exchange mapping work for DXLog contest fields.

## v0.1.30-v0.1.33

- True Interception-based exclusive Keyboard B routing.
- Private keystrokes marshalled directly to the standalone Second Operator UI thread.

## Earlier development

- DXLog native number-server reservation.
- Live QSO list, worked-band and SCP integration.
- VFO-B operation and dual PTT A/B support.
