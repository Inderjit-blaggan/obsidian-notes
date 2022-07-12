C:\Users\hp>`SCHTASKS /Create /SC ONCE  /ST 03:30:00 /TN slack_stop /TR "taskkill /f /im slack.exe"`
SUCCESS: The scheduled task "slack_stop" has successfully been created.

C:\Users\hp>`SCHTASKS /Create /SC ONCE  /ST 03:30:00 /TN Teams_stop /TR "taskkill /f /im teams.exe"`
SUCCESS: The scheduled task "Teams_stop" has successfully been created.

https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/schtasks-create

https://superuser.com/questions/77664/automatically-close-program-at-a-scheduled-time-each-day 



How to reduce the nat gateway cost?

