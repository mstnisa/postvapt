# PostVAPT

**From VAPT findings to verified fixes.**

PostVAPT turns vulnerability scanner reports into a prioritised, tracked and
verified remediation plan, with compliance evidence for **PCI DSS v4.0** and the
**Bangladesh Bank ICT Security Guideline v4.0**. It runs entirely on your own
computers: your scan data never leaves your network.

![Downloads](https://img.shields.io/github/downloads/mstnisa/postvapt/total?label=downloads)
![Latest release](https://img.shields.io/github/v/release/mstnisa/postvapt?label=release)

## What it does

- **Imports your scanners:** Nessus, OpenVAS / Greenbone, Burp Suite, OWASP ZAP,
  Nmap, Nuclei, SARIF (Trivy, Grype, Semgrep, CodeQL...) and CSV exports
  (Qualys, Rapid7, Acunetix, Invicti and others).
- **Decides what to fix first:** a transparent, deterministic SSVC decision tree
  using EPSS, the CISA Known Exploited Vulnerabilities catalogue and your asset
  context. Every decision shows why it was made.
- **Tracks every finding across scans:** new, persisting, reappeared and fixed,
  with SLA due dates.
- **Keeps people in control:** downgrades and risky decisions wait for an
  analyst's review, and every action is written to an audit log.
- **Confirms fixes safely:** non-destructive retests (TLS version, HTTP security
  headers, service banners, SMB signing) before a finding is closed.
- **Produces compliance evidence:** findings mapped to PCI DSS v4.0 and
  Bangladesh Bank ICT Security Guideline v4.0 controls, exported as an
  executive evidence pack (HTML, Markdown, JSON).
- **Works the way you do:** web dashboard, command line and a JSON API for
  automation (n8n, CI pipelines). Solo use or a team with roles.
- **Optional AI assistance:** an LLM (Groq, OpenAI or a local Ollama model) can
  extract evidence from findings. It never decides priority, and IP addresses
  are removed before anything is sent to a cloud provider. Off by default.

## Editions

| | Free edition | Paid edition |
|---|---|---|
| Assets (distinct hosts) | up to 50 | unlimited |
| User accounts | up to 3 | unlimited |
| PostgreSQL database | - | yes |
| Price | free | coming later |

Free edition. A paid edition will be offered later. Free keys already issued
keep working.

## Get started

### 1. Get a free license key

Fill in the **[license key request form](https://docs.google.com/forms/d/e/1FAIpQLSd--0kYvYrB2WO6oxgv-_bCoI7tWw8U95I-gF2Qh9Isnc6e_Q/viewform)**.
Your key is e-mailed to you, usually within one business day.

You can try PostVAPT before your key arrives: the offline demo needs no key.

### 2. Download

Get the latest version from the **[Releases page](https://github.com/mstnisa/postvapt/releases/latest)**:

| Platform | File |
|---|---|
| Windows 10/11 (64-bit) | `postvapt-<version>-windows-x86_64.zip` |
| Linux (64-bit, glibc 2.41 or newer, e.g. Debian 13, Ubuntu 25.04) | `postvapt-<version>-linux-x86_64.zip` |
| Docker (any Linux server) | `postvapt-<version>-docker.tar.gz` |

No Python or other software needs to be installed.

### 3. Install and run

**Windows**

1. Extract the zip to a folder, for example `C:\PostVAPT`.
2. Double-click **`Start PostVAPT.bat`**. The dashboard opens in your browser at
   http://127.0.0.1:8000.
3. Open the **License** page, paste your key and press **Activate**.

Windows SmartScreen may warn about a new, unsigned program. Choose
**More info > Run anyway** if you downloaded the file from this repository.

To try the demo first, open a terminal in the folder and run:

```
postvapt.exe demo --serve
```

**Linux**

```bash
unzip postvapt-<version>-linux-x86_64.zip -d ~/postvapt
cd ~/postvapt/postvapt
./postvapt-bin license activate <your-key>
./postvapt-bin serve --open
```

**Docker**

```bash
docker load -i postvapt-<version>-docker.tar.gz
docker run --rm -v postvapt-data:/data postvapt:<version> license activate <your-key>
docker run --rm -it -v postvapt-data:/data postvapt:<version> user add admin --role admin
docker run -d --name postvapt -p 8000:8000 -v postvapt-data:/data postvapt:<version>
```

The container serves the network, so it needs a user account: create the admin
account (second command) before starting it. Put PostVAPT behind HTTPS (a
reverse proxy or `--ssl-certfile` / `--ssl-keyfile`) before other people use it.

## Your first scan

On the dashboard: **Scans > Upload**, choose your scanner export and tick
**Run triage**. Or from the command line:

```bash
postvapt asset-import assets.csv            # optional: host, criticality, exposure, owner
postvapt ingest scan.nessus --scope dmz
postvapt triage
postvapt report --organisation "My Organisation"
```

`postvapt --help` lists every command.

## Privacy and network use

PostVAPT runs on your computers and stores everything in a local database.
It only uses the network when you use a feature that needs it:

| Feature | Destination | What is sent |
|---|---|---|
| Live threat intelligence (can be turned off) | api.first.org (EPSS), www.cisa.gov (KEV) | CVE IDs only |
| AI evidence extraction (off by default) | the provider you configure | finding text, with IP addresses removed for cloud providers |
| Retest (you start it) | the host of the finding | a non-destructive protocol check |
| Usage signal (only if you turn it on) | the PostVAPT developer | license ID, version, edition, OS family |

The license key is checked offline. PostVAPT works on networks with no internet
access (`--offline`).

Run retests only against systems you are authorised to test.

## Support

Questions, problems and feature requests: open an issue on the
**[Issues page](https://github.com/mstnisa/postvapt/issues)**. Please do not
post scan results, host names or other sensitive data in an issue.

## License

PostVAPT is proprietary software, provided under the
**[PostVAPT Evaluation License](LICENSE)**: you may install and use it for
internal evaluation and send feedback. Copying, redistribution, modification
and reverse engineering are not permitted. The source code is not published.

Open-source components included in the downloads keep their own licences; see
[THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES) and the `licenses` folder in each
download.

Copyright (c) 2026 Mst. Awalunnisa. All rights reserved.
