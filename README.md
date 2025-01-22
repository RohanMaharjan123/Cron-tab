# Crontab Guide and Commands

## Understanding Cron and Crontab

A comprehensive guide to understanding and using crontab in Unix-like operating systems.

Cron is a time-based job scheduler in Unix-like operating systems. Crontab (CRON TABle) is a configuration file that specifies shell commands to run periodically on a schedule.

## Table of Contents

- [Understanding Cron](#understanding-cron)
- [Cron Expression Format](#cron-expression-format)
- [Basic Crontab Commands](#basic-crontab-commands)
- [Special Characters](#special-characters)
- [Common Examples](#common-examples)
- [Special Strings](#special-strings)
- [Advanced Usage](#advanced-usage)
- [Best Practices](#best-practices)

## Understanding Cron

Cron is a time-based job scheduler in Unix-like operating systems. Crontab (CRON TABle) is a configuration file that specifies shell commands to run periodically on a given schedule.

## Cron Expression Format

```bash
Min  Hour Day  Mon  Weekday  Command
*    *    *    *    *       command to be executed

┬    ┬    ┬    ┬    ┬
│    │    │    │    └─  Day of Week   (0=Sun .. 6=Sat)
│    │    │    └──────  Month         (1..12)
│    │    └───────────  Day of Month  (1..31)
│    └────────────────  Hour          (0..23)
└─────────────────────  Minute        (0..59)
```

### Field Descriptions

| Field         | Range  | Special Characters     |
|---------------|--------|------------------------|
| Minute        | 0 - 59 | `, - * /`              |
| Hour          | 0 - 23 | `, - * /`              |
| Day of Month  | 1 - 31 | `, - * ? / L W`        |
| Month         | 1 - 12 | `, - * /`              |
| Day of Week   | 0 - 6  | `, - * ? / L #`        |

## Basic Crontab Commands

|         Command          |             Description                                    |
|--------------------------|------------------------------------------------------------|
| `crontab -e`             | Edit or create your crontab file                           |
| `crontab -l`             | Display your crontab file                                  |
| `crontab -r`             | Remove your crontab file                                   |
| `crontab -v`             | Display the last time you edited your crontab file         |
| `crontab -u username -l` | View another user's crontab (requires root)                |
| `crontab -i -r`          | Remove crontab with confirmation prompt                    |
| `crontab file.txt`       | Install a new crontab from file                            |
| `crontab -u username -e` | Edit another user's crontab (requires root)                |

## Special Characters

| Character | Description | Example |
|-----------|-------------|----------|
| `*` | Matches all values | `* * * * *` runs every minute |
| `-` | Range of values | `1-5` means 1,2,3,4,5 |
| `/` | Step values | `*/15` means every 15 units |
| `,` | List of values | `1,3,5` means 1, 3, and 5 |
| `L` | Last day of month/week | `1L` means last Sunday |
| `#` | Nth day of month | `5#3` means 3rd Friday |
| `?` | No specific value | Used instead of `*` for Day of Month/Week |
| `W` | Nearest weekday | `15W` means nearest weekday to 15th |

## Common Examples

| Expression         | Description                                         |
|--------------------|-----------------------------------------------------|
| `*/15 * * * *`     | Every 15 minutes                                    |
| `0 * * * *`        | Every hour                                          |
| `0 */2 * * *`      | Every 2 hours                                       |
| `15 2 * * *`       | At 2:15 AM every day                                |
| `10 9 * * 5`       | At 9:10 AM every Friday                             |
| `0 0 * * 0`        | At midnight every Sunday                            |
| `15 2 * * 1L`      | At 2:15 AM on the last Monday of every month        |
| `0 0 1 * *`        | Every 1st of the month                              |
| `*/5 9-17 * * 1-5` | Every 5 min, 9 AM-5 PM, Monday-Friday               |
| `0 22 * * 1-5`     | At 10 PM on weekdays                                |
| `23 0-20/2 * * *`  | At minute 23 past every 2nd hour from 0-20          |
| `@reboot`          | At system startup                                   |

## Special Strings

| String       | Equivalent Expression       | Description                     |
|--------------|-----------------------------|---------------------------------|
| `@reboot`    |                             | Run once, at system startup     |
| `@yearly`    | `0 0 1 1 *`                 | Run once every year            |
| `@monthly`   | `0 0 1 * *`                 | Run once every month           |
| `@weekly`    | `0 0 * * 0`                 | Run once every week            |
| `@daily`     | `0 0 * * *`                 | Run once each day              |
| `@hourly`    | `0 * * * *`                 | Run once an hour               |

## Advanced Usage

### Environment Variables in Crontab

```bash
# Set environment variables at the top of crontab
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
MAILTO=user@example.com

# Use environment variables in commands
0 5 * * * $HOME/backup.sh > $HOME/logs/backup.log 2>&1
```

### Logging Cron Jobs

```bash
# Log with timestamp
* * * * * /path/to/script.sh >> /var/log/script.log 2>&1

# Log with date prefix
* * * * * (date; /path/to/script.sh) >> /var/log/script.log 2>&1
```

## Best Practices

1. Always use absolute paths in crontab
2. Redirect output to prevent email notifications:

```bash
   0 5 * * * /path/to/script.sh > /dev/null 2>&1
```

3. Add a MAILTO variable to receive job notifications
4. Test scripts separately before adding to crontab
5. Use comments to document complex schedules
6. Include error handling in your scripts
7. Set appropriate environment variables
8. Use proper log rotation for log files

## Troubleshooting

Common issues and solutions:

1. Check cron daemon status:

```bash
   sudo service cron status
```

2. View cron logs:

```bash
    sudo grep CRON /var/log/syslog
```

3. Ensure proper permissions:

```bash
    chmod +x /path/to/script.sh
```

4. Verify script paths and environment variables

For more detailed information, refer to the official [cron documentation](https://man7.org/linux/man-pages/man5/crontab.5.html).
