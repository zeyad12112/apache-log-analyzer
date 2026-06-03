# 🪵 Apache Log Analyzer — Terminal-Bench Task

A self-contained **Bash/Linux coding challenge** built to the [terminal-bench](https://github.com/laude-institute/terminal-bench) task specification.

Given an Apache access log, the agent must produce a structured report using only standard Unix tools.

---

## 📋 Task Description

The agent is placed in a Docker container with `/app/access.log` — a realistic Apache access log. It must produce `/app/report.txt` containing:

1. **Top 3 IPs** by request count
2. **Total 404 errors**
3. **Requests per hour** breakdown

Only standard Unix tools are allowed: `awk`, `grep`, `sort`, `uniq`, `sed`.

---

## 📁 File Structure

```
apache-log-analyzer/
├── Dockerfile               # Ubuntu 22.04 environment
├── docker-compose.yaml      # Client service definition
├── task.yaml                # Task metadata & description
├── access.log               # Sample Apache access log (25 entries)
├── solution.sh              # Reference oracle solution
├── run-tests.sh             # Test runner (pytest via uv)
└── tests/
    └── test_outputs.py      # 9 pytest tests
```

---

## ✅ Test Coverage (9 tests)

| Test | What it checks |
|------|---------------|
| `test_report_file_exists` | `/app/report.txt` was created |
| `test_top_ips_section_present` | `=== TOP 3 IPs ===` header exists |
| `test_top_ip_is_correct` | Top IP is `192.168.1.10` with 8 requests |
| `test_top_3_ips_correct` | All 3 IPs and counts are correct |
| `test_404_section_present` | `=== 404 REQUESTS ===` header exists |
| `test_404_count_correct` | Exactly 4 404 errors |
| `test_requests_per_hour_section_present` | `=== REQUESTS PER HOUR ===` header exists |
| `test_requests_per_hour_correct` | Correct hourly breakdown (08–13) |
| `test_no_extra_sections` | No extra/unexpected sections |

---

## 🔬 How to Run Locally

### Prerequisites
- Docker
- Python 3.12
- [uv](https://github.com/astral-sh/uv)
- terminal-bench installed:

```bash
git clone https://github.com/laude-institute/terminal-bench.git
cd terminal-bench
uv sync
```

### Run oracle agent (should pass 100%)
```bash
uv run tb run --agent oracle --task-id apache-log-analyzer
```

### Run nop agent (should pass 0%)
```bash
uv run tb run --agent nop --task-id apache-log-analyzer
```

### Interactive debugging
```bash
uv run tb tasks interact -t apache-log-analyzer
```

---

## 🧩 Expected Output (`/app/report.txt`)

```
=== TOP 3 IPs ===
192.168.1.10 8
10.0.0.5 7
192.168.1.20 4

=== 404 REQUESTS ===
4

=== REQUESTS PER HOUR ===
08 3
09 5
10 6
11 4
12 4
13 3
```

---

## 🐳 Docker Environment

- Base image: `ubuntu:22.04`
- Tools available: `awk`, `grep`, `sed`, `sort`, `uniq`, `coreutils`, `tmux`, `asciinema`
- Working directory: `/app`
- Input file: `/app/access.log`

---

## 👤 Author

**Zeyad Aymen** — [github.com/zeyad12112](https://github.com/zeyad12112)
