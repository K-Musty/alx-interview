 Log Parsing Script

## Overview

This script reads lines from standard input (stdin) and computes metrics based on the input format. The metrics include the total file size and the count of specific HTTP status codes.

## Input Format

The expected input format for each log line is:

<IP Address> - [<date>] "GET /projects/260 HTTP/1.1" <status code> <file size>

markdown


Lines that do not match this format will be skipped.

## Metrics

The script computes the following metrics:
- **Total file size:** The sum of all `<file size>` values from the input.
- **Number of lines by status code:** The count of occurrences for each of the following status codes: `200`, `301`, `400`, `401`, `403`, `404`, `405`, `500`.

## Output

The script prints the metrics after every 10 lines and upon receiving a keyboard interruption (CTRL + C).

### Example Output

File size: 12345
200: 10
301: 2
404: 3

markdown


## Usage

1. Save the script to a file, e.g., `log_parser.py`.
2. Run the script and provide input through stdin. For example:
   ```bash
   python3 log_parser.py < logfile.log

Script Code

python

#!/usr/bin/python3
"""Reads from standard input and computes metrics.

After every ten lines or the input of a keyboard interruption (CTRL + C),
prints the following statistics:
    - Total file size up to that point.
    - Count of read status codes up to that point.
"""


def print_stats(size, status_codes):
    """Print accumulated metrics.

    Args:
        size (int): The accumulated read file size.
        status_codes (dict): The accumulated count of status codes.
    """
    print("File size: {}".format(size))
    for key in sorted(status_codes):
        print("{}: {}".format(key, status_codes[key]))

if __name__ == "__main__":
    import sys

    size = 0
    status_codes = {}
    valid_codes = ['200', '301', '400', '401', '403', '404', '405', '500']
    count = 0

    try:
        for line in sys.stdin:
            if count == 10:
                print_stats(size, status_codes)
                count = 1
            else:
                count += 1

            line = line.split()

            try:
                size += int(line[-1])
            except (IndexError, ValueError):
                pass

            try:
                if line[-2] in valid_codes:
                    if status_codes.get(line[-2], -1) == -1:
                        status_codes[line[-2]] = 1
                    else:
                        status_codes[line[-2]] += 1
            except IndexError:
                pass

        print_stats(size, status_codes)

    except KeyboardInterrupt:
        print_stats(size, status_codes)
        raise

Handling Keyboard Interruption

The script handles keyboard interruptions (CTRL + C) gracefully by printing the metrics before exiting.
