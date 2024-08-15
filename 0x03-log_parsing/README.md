# 0x03-log_parsing

## Overview

This script reads lines from standard input (stdin) and computes metrics based on the input format. The metrics include the total file size and the count of specific HTTP status codes.

## Input Format

The expected input format for each log line is:

<IP Address> - [<date>] "GET /projects/260 HTTP/1.1" <status code> <file size>


