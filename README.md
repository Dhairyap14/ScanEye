# ScanEye
ScanEye is a simple yet powerful malware scanner built with Rust, leveraging YARA rules for comprehensive file analysis and real-time monitoring of filesystem activity.

## Features
Real-time scanning: Monitors created and modified files using platform-specific tools:
  Linux: inotify
  macOS: FSEvents
  Windows: ReadDirectoryChanges
  Other platforms: Polling
Full YARA rule support for flexible and robust malware detection.
Single scan mode for one-time folder analysis with detailed reporting.
Parallel scanning using a configurable thread pool for efficiency.
Multiple reporting formats: log, text, and JSON.

## Known Limitations
While ScanEye is lightweight and non-invasive, its design prioritizes simplicity over advanced functionality. This introduces some limitations:
    Files locked exclusively by other processes may result in "Permission Denied" errors during scanning.
    Malicious file creation or execution is not prevented but will be reported.
    Fileless malware detection is not supported.
    Links between detected files and their originating processes are unavailable.

## Building
To build ScanEye, run the following command:
```sh
cargo build --release
```
## Dependencies
Ensure your system has libssl-dev installed. On Ubuntu-based systems, you can install it with:
```sh
sudo apt install libssl-dev
```

## Running
Assuming you have your YARA rules in ./yara-rules, run the following command to start ScanEye:

```sh
sudo ./target/release/scaneye --rules ./yara-rules
```

## Single Scan
For a one-time recursive scan of a specified folder, use the --scan option:
```sh
sudo ./target/release/scaneye --rules ./yara-rules --scan --root /path/to/scan
```
You can specify which file extensions to scan (default is all files) using the --ext argument:

```sh
sudo ./target/release/scaneye \
    --rules ./yara-rules \
    --scan \
    --root /path/to/scan \
    --ext exe \
    --ext elf \
    --ext doc \
    --ext docx
```

## Reporting
Various options are available for reporting:
--report-clean: Include clean files in the report.
--report-errors: Highlight errors explicitly in the output (errors are logged by default).
--report-output <FILENAME>: Save the scan results to a specified file.
--report-json: Use JSON format for reports when combined with --report-output.

## Additional Options
```sh
scaneye --help   for the complete list of options.
```
