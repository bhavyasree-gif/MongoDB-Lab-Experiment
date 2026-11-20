# MongoDB Lab Experiments

**Student:** Bhavya Sree  
**USN:** 24BTRCL110  
**Programme/Section:** CSE – AIML (B)  
**Course:** NoSQL Database using MongoDB

## Lab 14 – MongoDB Monitoring, Backup and Restore

This repository contains two Python implementations:

- **Lab 14.1 – Monitoring Dashboard:** collects MongoDB server metrics, displays them in a terminal dashboard, records metrics in a log, and supports optional webhook alerts.
- **Lab 14.2 – Automated Backup and Restore:** creates timestamped backups, restores a dedicated test collection, verifies data using document counts and SHA-256 checksums, benchmarks dataset sizes, and supports hourly backups with seven-day retention.

## Repository layout

```text
.
├── README.md
├── .gitignore
├── lab_14_1_monitoring/
│   ├── monitoring_dashboard.py
│   ├── requirements.txt
│   └── .env.example
└── lab_14_2_backup_restore/
    ├── backup_verify.py
    ├── requirements.txt
    └── .env.example
```

## Requirements

- Python 3.10+
- MongoDB server (local or Atlas)
- MongoDB Database Tools (`mongodump` and `mongorestore`) on PATH for Lab 14.2

## Lab 14.1 – Monitoring

From `lab_14_1_monitoring`:

```bash
python -m venv .venv
# Windows PowerShell: .venv\Scripts\Activate.ps1
# macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
```

Set your connection string locally (never commit credentials):

```powershell
$env:MONGO_URI = "mongodb://localhost:27017"
```

Run:

```bash
python monitoring_dashboard.py
```

The dashboard samples every 30 seconds by default, showing connection counts, operation counters, WiredTiger cache metrics, and replica-set lag when available. Logs are written to `logs/metrics.log`. Optional environment variables: `MONITOR_INTERVAL_SECONDS` and `ALERT_WEBHOOK_URL`.

## Lab 14.2 – Backup and restore

From `lab_14_2_backup_restore`:

```bash
python -m venv .venv
# Windows PowerShell: .venv\Scripts\Activate.ps1
# macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
```

Set `MONGO_URI`, then run:

```bash
python backup_verify.py --mode verify --count 1000
python backup_verify.py --mode benchmark --counts 1000,10000,100000
python backup_verify.py --mode schedule
```

The scheduler performs an initial backup and then runs hourly, retaining seven days of timestamped backup folders.

## Safety

The backup/restore script is restricted to database `lab14_backup_restore` and collection `backup_test`. It uses `--drop` only on that test namespace. Do not point this educational exercise at production data. Do not commit `.env`, passwords, webhook URLs, virtual environments, logs, or generated backup files. The benchmark clears and recreates data in the dedicated test collection.

## Author

Bhavya Sree — 24BTRCL110
