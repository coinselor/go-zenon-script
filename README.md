# Zenon.sh: Zenon Network (`go-zenon`) Installation and Monitoring Scripts

This repository contains scripts to automate the installation, deployment, and monitoring of the **Zenon Network** node (`go-zenon`) with comprehensive monitoring using **Prometheus**, **Node Exporter**, and **Grafana**.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Features](#features)
3. [Installation](#installation)
4. [Script Usage](#script-usage)
   - [go-zenon.sh](#go-zenonsh)
   - [grafana.sh](#grafanash)
5. [Monitoring Setup](#monitoring-setup)
6. [Systemd Service for go-zenon](#systemd-service-for-go-zenon)
7. [Troubleshooting](#troubleshooting)
8. [License](#license)

## Prerequisites

Before running the scripts, ensure you have:

- A Linux-based operating system (preferably Ubuntu or Debian).
- Root access (required for system changes, package installation, and systemd configuration).
- Git installed (`sudo apt-get install git`).

The `go-zenon.sh` script requires the following software, which it will install automatically if not present:

- `curl`
- `make`
- `gcc`
- `jq`
- `Go 1.23.0`

## Features

- **Automated Installation**: Simplifies the installation of Go, necessary dependencies, and the go-zenon software.
- **Service Management**: Configures go-zenon as a systemd service for easy management (start, stop, restart, status).**
- **Monitoring Setup**: Installs and configures Prometheus, Node Exporter, and Grafana for real-time monitoring.
- **Branch Selection**: Allows users to select specific branches of the go-zenon repository for cloning and building.
- **Restoration**: Provides functionality to restore go-zenon from a bootstrap script.

## Installation

### Clone the Repository

```bash
git clone https://github.com/zenon-network/go-zenon-script.git
cd go-zenon-script
```

### Run the Installation Script

To deploy the Zenon Network node:

```bash
sudo ./go-zenon.sh --deploy
```

This command will:

1. Install necessary dependencies.
2. Download and install the Go programming language.
3. Clone the `go-zenon` repository and build the project.
4. Set up `go-zenon` as a systemd service and start the service.

## Script Usage

### go-zenon.sh

The `go-zenon.sh` script automates the setup and management of a Zenon Network node. It provides several options to manage the lifecycle of the node.

#### Deploy the Node

To deploy the `go-zenon` node from scratch:

```bash
sudo ./go-zenon.sh --deploy
```

#### Restore from Bootstrap

If you need to restore the node from a bootstrap file:

```bash
sudo ./go-zenon.sh --restore
```

#### Start/Stop/Restart the Service

- To start the `go-zenon` service:

  ```bash
  sudo ./go-zenon.sh --start
  ```

- To stop the `go-zenon` service:

  ```bash
  sudo ./go-zenon.sh --stop
  ```

- To restart the `go-zenon` service:

  ```bash
  sudo ./go-zenon.sh --restart
  ```

#### Monitor Node Logs

You can monitor the `znnd` logs:

```bash
sudo ./go-zenon.sh --status
```

This command will display the live logs from the system's `syslog` filtered for `znnd` events.

### grafana.sh

The `grafana.sh` script sets up monitoring for the Zenon Network node. It installs and configures the following:

- **Prometheus**: for scraping metrics.
- **Node Exporter**: for collecting system metrics.
- **Grafana**: for visualizing metrics using a pre-configured dashboard.

#### Install Monitoring Tools

Run the following command to install and configure Prometheus, Node Exporter, and Grafana:

```bash
sudo ./grafana.sh
```

This will:

1. Install **Prometheus** and **Node Exporter** to scrape and collect system metrics.
2. Install **Grafana** and configure it to use Prometheus as its data source.
3. Import a pre-built **Prometheus Node Exporter Dashboard** into Grafana for easy visualization of system metrics.

After installation, you can access the Grafana UI on:

```
http://localhost:3000
```

(Default credentials: `admin/admin`)

## Monitoring Setup

After the `grafana.sh` script runs, your system will have a full monitoring stack that includes:
- **Node Exporter**: A lightweight agent that exposes system metrics.
- **Prometheus**: Collects metrics from Node Exporter and other sources.
- **Grafana**: A powerful visualization tool for monitoring system metrics.

You can log in to Grafana and explore the **Prometheus Node Exporter Dashboard**, which provides insights into your system's CPU, memory, disk, and network usage.

## Systemd Service for go-zenon

The `go-zenon` node is installed as a **systemd** service, making it easy to manage. The service file is located at `/etc/systemd/system/go-zenon.service`.

### Commands to Manage the Service

- **Enable the service** (starts on boot):

  ```bash
  sudo systemctl enable go-zenon
  ```

- **Check the status of the service**:

  ```bash
  sudo systemctl status go-zenon
  ```

- **View logs for the service**:

  ```bash
  sudo journalctl -u go-zenon
  ```

## Troubleshooting

### Common Issues

1. **Go not installed properly**: Ensure that the script has successfully installed Go. You can verify this by running:

   ```bash
   go version
   ```

2. **Missing dependencies**: The script attempts to install all necessary dependencies. If any issues occur during installation, verify that `make`, `gcc`, and `jq` are installed correctly.

3. **Service not starting**: Check the status and logs of the `go-zenon` service:

   ```bash
   sudo systemctl status go-zenon
   sudo journalctl -u go-zenon
   ```

4. **Grafana not accessible**: Make sure Grafana is running by checking the service status:

   ```bash
   sudo systemctl status grafana-server
   ```

### Logs

- For `go-zenon` service logs:

  ```bash
  sudo journalctl -u go-zenon
  ```

- For system monitoring logs (Prometheus, Node Exporter, and Grafana):

  ```bash
  sudo journalctl -u prometheus
  sudo journalctl -u node_exporter
  sudo journalctl -u grafana-server
  ```

## License

This project is licensed under the GNU General Public License v3.0. You are free to copy, modify, and distribute the software under the same license. However, there is no warranty for the software. See the [LICENSE](LICENSE) file for more details.
