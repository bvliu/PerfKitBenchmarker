# PerfKit Benchmarker

![PerfKit Benchmarker Logo](docs/img/pkb-logo@96w.png)

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

PerfKit Benchmarker is an open effort to define a canonical set of benchmarks to measure and compare cloud offerings. It's designed to operate via vendor provided command line tools, ensuring consistency across services without vendor-specific tuning.

---

## 🚀 Quick Start

Get running in 3 minutes:

```bash
# Clone the repository
git clone https://github.com/GoogleCloudPlatform/PerfKitBenchmarker.git
cd PerfKitBenchmarker

# Install dependencies
pip install -r requirements.txt

# Run a simple benchmark (e.g., iperf on GCP)
./pkb.py --project=<your-project> --benchmarks=iperf --machine_type=f1-micro
```

---

## 📖 Documentation

Choose your path based on your needs:

### 🛠 [User Manual (GitHub Pages)](https://googlecloudplatform.github.io/PerfKitBenchmarker/)
*Polished, public-facing guides for end-users.*
- [Installation Guide](https://googlecloudplatform.github.io/PerfKitBenchmarker/installation/)
- [Usage Examples](https://googlecloudplatform.github.io/PerfKitBenchmarker/usage/)
- [Configuration Reference](https://googlecloudplatform.github.io/PerfKitBenchmarker/configuration/)
- [Benchmarks & Licensing](https://googlecloudplatform.github.io/PerfKitBenchmarker/benchmarks/)

### 🏗 [Internal Blueprint (Wiki)](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker/wiki)
*Technical details and evolving documentation for contributors.*
- [Architecture Notes](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker/wiki/Design-Docs)
- [Project Roadmap](https://googlecloudplatform.github.io/PerfKitBenchmarker/internal/roadmap/)
- [Contribution Standards](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker/blob/master/CONTRIBUTING.md)
- [FAQ](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker/wiki/FAQ)

### 🤝 Contributing
Please see our [CONTRIBUTING.md](./CONTRIBUTING.md) for details on how to get involved.

---

*If it takes more than 3 minutes to read, it’s probably too long. For deep dives, visit our [full documentation](https://googlecloudplatform.github.io/PerfKitBenchmarker/).*
