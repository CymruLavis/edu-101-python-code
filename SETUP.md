# Personal Issues
I ran into some issues during the set up of the temporal development environment. When syncing my virtual environment with the requirements.txt file and starting the dev server, "temporal" was not a recognized command. The installed dependancy "temporalio" is a separate package.

## Solution
To fix this I had to manually install the Temporal CLI for windows using the correct download from the most recent release page. https://github.com/temporalio/cli/releases  


Step 1: Download the correct zip file - i needed windows amd64
```
$zipPath = "$env:TEMP\temporal_cli.zip"
Invoke-WebRequest -Uri https://github.com/temporalio/cli/releases/download/v1.4.1/temporal_cli_1.4.1_windows_amd64.zip -OutFile $zipPath
```

Step 2: Extract the zip contents to my local .temporal\bin folder which I created manually in "Users\USER_PROFILE" directory
```
$extractPath = "$env:USERPROFILE\.temporal\bin"
Expand-Archive -Path $zipPath -DestinationPath $extractPath -Force
```

Step 3: Verify the executable "temporal.exe" existed
```
Get-ChildItem $extractPath\temporal.exe
```

Step 4: Add ".temporal\bin" to my PATH variables
```
$temporalPath = "$env:USERPROFILE\.temporal\bin"
$oldPath = [Environment]::GetEnvironmentVariable("PATH", "User")
if (-not $oldPath.Contains($temporalPath)) {
    [Environment]::SetEnvironmentVariable("PATH", "$oldPath;$temporalPath", "User")
}
```
Step 5: Restart VSCode and terminals so everything could refresh

Step 6: Test that the Temporal CLI was installed
```
temporal version
```

Finally I was able to run the dev server command
```
temporal server start-dev --ui-port 8080
```