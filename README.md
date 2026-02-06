# Netops Weekly Vulnerability Report Generator

A Python script that compares weekly Tenable vulnerability exports and generates a comprehensive Word document report for security standups and management review.

## Features

- **Week-over-Week Comparison**: Compares two CSV vulnerability exports to show changes
- **Vendor Breakdown**: Categorizes vulnerabilities by vendor (Cisco, Palo Alto, Other) using hostname patterns
- **Severity Analysis**: Breaks down vulnerabilities by Critical, High, Medium, Low
- **Color-Coded Changes**: Red for increases (bad), Green for decreases (good)
- **Visual Charts**: Severity comparison bar chart, change summary chart, vendor pie chart
- **KEV Analysis**: Identifies CISA Known Exploited Vulnerabilities in your environment (optional)
- **CVE Change Analysis**: Explains week-over-week changes by listing specific devices added/removed
- **Hyperlinked CVEs**: CVE IDs link directly to CVE.org for quick reference
- **Professional Formatting**: Table of Contents, page breaks, consistent styling

## Requirements

### Python Version
- Python 3.8 or higher

### Dependencies
Install required packages:
```bash
pip install pandas python-docx matplotlib
```

## Files

| File | Description |
|------|-------------|
| `weekly_vuln_report.py` | Main report generation script |
| `update_kev_list.py` | Downloads CISA KEV catalog (optional) |
| `kev_catalog.json` | CISA KEV data file (created by update_kev_list.py) |

## Usage

### Basic Usage

```bash
python weekly_vuln_report.py <previous_week.csv> <current_week.csv> [output_name]
```

### Example

```bash
python weekly_vuln_report.py "vulnerabilities-2025-01-27.csv" "vulnerabilities-2025-02-03.csv" "Weekly_Report_Feb3"
```

This generates `Weekly_Report_Feb3.docx` in the current directory.

### Workflow

1. **Export from Tenable**: Download the current week's vulnerability CSV export from Tenable
2. **Run the Script**: Compare the new export with the previous week's export
3. **Review Report**: Open the generated .docx file in Microsoft Word
4. **Present**: Use for weekly security standups or management review

## CSV File Requirements

The script expects Tenable vulnerability export CSV files with these columns:
- `asset.host_name` - Device hostname
- `asset.id` - Unique asset identifier
- `asset.name` - Asset name
- `definition.id` - Vulnerability definition ID
- `definition.name` - Vulnerability name
- `definition.cve` - CVE identifier(s)
- `definition.family` - Vulnerability family
- `definition.vpr.score` - VPR score
- `severity` - Severity level (Critical, High, Medium, Low)

## KEV Analysis (Optional)

To include CISA Known Exploited Vulnerabilities analysis:

### Option 1: Automatic Download
```bash
python update_kev_list.py
```

### Option 2: Manual Download
1. Visit: https://www.cisa.gov/known-exploited-vulnerabilities-catalog
2. Click "Download" and select JSON format
3. Save as `known_exploited_vulnerabilities.json` in the same folder as the script

The report will automatically include KEV analysis if the catalog file is present.

---

## Platform-Specific Instructions

### macOS

#### Prerequisites
1. **Install Python** (if not already installed):
   ```bash
   # Using Homebrew
   brew install python
   ```

2. **Install dependencies**:
   ```bash
   pip3 install pandas python-docx matplotlib
   ```

#### Permissions
- **File Access**: Ensure you have read/write permissions in the directory where you're running the script
- **Downloads Folder**: If running from Downloads, you may need to grant Terminal access:
  - System Preferences → Security & Privacy → Privacy → Files and Folders
  - Add Terminal and enable access to Downloads

#### Running the Script
```bash
cd /path/to/your/scripts
python3 weekly_vuln_report.py "previous_week.csv" "current_week.csv" "Report_Name"
```

#### Troubleshooting macOS
- If you see "permission denied", try: `chmod +x weekly_vuln_report.py`
- If matplotlib fails, install via: `pip3 install matplotlib --user`

---

### Windows

#### Prerequisites
1. **Install Python**:
   - Download from https://www.python.org/downloads/
   - **Important**: Check "Add Python to PATH" during installation

2. **Install dependencies** (Command Prompt or PowerShell):
   ```cmd
   pip install pandas python-docx matplotlib
   ```

#### Permissions
- **Run as Administrator**: If you encounter permission errors, right-click Command Prompt and select "Run as administrator"
- **Execution Policy** (PowerShell only): If scripts are blocked:
  ```powershell
  Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
  ```

#### Running the Script
```cmd
cd C:\path\to\your\scripts
python weekly_vuln_report.py "previous_week.csv" "current_week.csv" "Report_Name"
```

#### File Paths with Spaces
If your file paths contain spaces, wrap them in quotes:
```cmd
python weekly_vuln_report.py "C:\Users\Name\My Documents\vuln-jan27.csv" "C:\Users\Name\My Documents\vuln-feb03.csv" "Weekly_Report"
```

#### Troubleshooting Windows
- If `python` is not recognized, use `py` instead: `py weekly_vuln_report.py ...`
- If pip fails, try: `python -m pip install pandas python-docx matplotlib`

---

## Report Sections

The generated report includes:

1. **Cover Page** - Title, date range, generation timestamp
2. **Table of Contents** - Quick navigation
3. **Executive Summary** - Overall changes with charts
4. **Breakdown by Severity** - Critical/High/Medium/Low counts
5. **Breakdown by Vendor** - Cisco, Palo Alto, Other with pie chart
6. **Detailed Breakdown** - Vendor × Severity matrix
7. **Top Vulnerability Changes** - Biggest increases/decreases per vendor
8. **Current Top Vulnerabilities** - Most impactful CVEs by asset count
9. **Asset Risk Analysis** - Most vulnerable assets by VPR score
10. **Assets with Critical Vulnerabilities** - Priority remediation list
11. **KEV Analysis** (if catalog present) - Known exploited vulnerabilities
12. **Definition & Asset Changes** - New/removed vulnerabilities and assets
13. **CVE Change Analysis** - Device-level explanation of changes

## Customization

### Device Categorization
The script categorizes devices by hostname patterns. To modify:
- Edit the `categorize_device_type()` function
- Hostname patterns are checked first, then `definition.family` as fallback

### Report Styling
- Colors are defined in the `COLORS` dictionary at the top of the script
- Table header colors can be changed in the `add_styled_table()` calls

## Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| "ModuleNotFoundError: No module named 'pandas'" | Run `pip install pandas` |
| "FileNotFoundError" | Check that CSV file paths are correct |
| "UnicodeDecodeError" | Ensure CSV files are UTF-8 encoded |
| Charts not appearing | Install matplotlib: `pip install matplotlib` |
| KEV section missing | Download KEV catalog (see KEV Analysis section) |

### Memory Issues with Large Files
For very large CSV files (>100MB), you may need to:
- Close other applications to free memory
- Process on a machine with more RAM

## Version History

- **v1.0** - Initial release with basic comparison
- **v2.0** - Added HexTracker-style hostname categorization
- **v3.0** - Added charts, color coding, and professional formatting
- **v4.0** - Added KEV analysis and CVE change analysis sections

## Support

For issues or feature requests, contact the NetOps Security Team.

---

*Generated reports are intended for internal security review purposes.*
