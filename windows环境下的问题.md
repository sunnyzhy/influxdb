# There was an error writing history file: open : The system cannot find the file specified.
Create batch file in the same folder as is influx.exe, name it for example influx.bat. Add following code inside the bat:

```
@ECHO OFF
SETLOCAL
SET HOME=%~dp0
"%~dp0\influx.exe" %*
ENDLOCAL
```
