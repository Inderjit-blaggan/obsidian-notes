#### Service level troubleshooting

- `pgrep <service-name`: This command is used to get the process id of the service
- `kill -SIGSTOP $(pgrep httpd)` : command used to **pause the service** gracefully, so it can restarted again from the same state.
- `kill -SIGCONT $(pgrep httpd)` : command used to **restart the service.**
- `kill -SIGTERM $(pgrep httpd)` : command used to **stop the service**.
- `kill -SIGKILL $(pgrep httpd)` : command used to **forcefully kill the service**. In numeric this is `-9` signal.