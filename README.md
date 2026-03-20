# CloudForms to QuickCharts Script

A Python script that queries a [Red Hat CloudForms](https://www.redhat.com/en/technologies/management/cloudforms) instance for cloud provider cost data and renders the results as an interactive stacked bar chart using [QuickChart](https://quickchart.io/).

## Overview

`total-cost-chart.py` retrieves a pre-existing CloudForms report named **"Total Cost Providers"** via the CloudForms REST API, extracts the last 7 days of cost data for each cloud provider (AWS, Azure, and vCenter/vSphere), and opens a bar chart in the default web browser.

## Requirements

- Python 3
- [requests](https://pypi.org/project/requests/)
- [quickchart-python](https://pypi.org/project/quickchart-python/)

Install the dependencies with:

```bash
pip install requests quickchart-python
```

## Configuration

Before running the script, update the following values inside `total-cost-chart.py`:

| Variable / Value | Description |
|---|---|
| `https://10.99.92.141` | URL of your CloudForms instance |
| `"admin"` | CloudForms username |
| `'passw0rd'` | CloudForms password |
| `num_days_report` | Number of days to include in the chart (default: `7`) |
| Date labels in `qc.config` | Update the date strings to match the period you want to display |

> **Note:** The script currently disables SSL certificate verification (`verify=False`). Update this for production use.

## Usage

```bash
python total-cost-chart.py
```

The script will:

1. Authenticate with the CloudForms REST API and obtain an auth token.
2. Fetch the latest result set for the **"Total Cost Providers"** report.
3. Extract and pad the last `num_days_report` days of cost data for **AWS**, **Azure**, and **vCenter**.
4. Build a stacked bar chart configuration via the QuickChart library.
5. Open the generated chart URL in your default web browser.

## Output

A stacked bar chart titled **"Total Cost by Providers"** is opened in the browser, showing daily costs broken down by:

- **Amazon (AWS)** – displayed in red
- **Azure** – displayed in amber/yellow
- **vCenter** – displayed in blue (costs are scaled down by a factor of 100)
