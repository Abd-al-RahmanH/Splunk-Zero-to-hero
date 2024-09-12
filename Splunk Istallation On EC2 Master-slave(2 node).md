---

# Splunk Installation: Zero-to-Hero Guide

This section will guide you through installing and configuring Splunk Enterprise (Master) and a Universal Forwarder (Slave) for data forwarding. We will also cover the key Splunk components and provide a brief introduction to writing queries in Splunk.

---

## Table of Contents
1. [Splunk Master Installation](#splunk-master-installation)
2. [Splunk Slave Installation (Universal Forwarder)](#splunk-slave-installation-universal-forwarder)
3. [Key Components of Splunk](#key-components-of-splunk)
4. [Configuration](#configuration)
   - [Master Configuration](#master-configuration)
   - [Slave Configuration](#slave-configuration)
5. [Adding Data Sources](#adding-data-sources)
6. [Running Queries in Splunk](#running-queries-in-splunk)

---

## Splunk Master Installation

To install Splunk Enterprise on your master server:

1. Navigate to the `/opt` directory:
   ```bash
   cd /opt
   ```
2. Download the Splunk package:
   ```bash
   wget -O splunk-8.2.6-a6fe1ee8894b-Linux-x86_64.tgz https://download.splunk.com/products/splunk/releases/8.2.6/linux/splunk-8.2.6-a6fe1ee8894b-Linux-x86_64.tgz
   ```
3. Extract the tarball and navigate to the `splunk/bin` directory:
   ```bash
   tar -xvzf splunk-8.2.6-a6fe1ee8894b-Linux-x86_64.tgz
   cd splunk/bin
   ```
4. Start Splunk:
   ```bash
   ./splunk start
   ```
5. Access the Splunk web interface by visiting the following URL in your browser:
   ```
   http://<your-server-ip>:8000
   ```

---

## Splunk Slave Installation (Universal Forwarder)

Install the Splunk Universal Forwarder on the slave machine to forward data to the master:

1. Navigate to the `/opt` directory:
   ```bash
   cd /opt
   ```
2. Download the Splunk Universal Forwarder:
   ```bash
   wget -O splunkforwarder-8.2.6-a6fe1ee8894b-Linux-x86_64.tgz https://download.splunk.com/products/universalforwarder/releases/8.2.6/linux/splunkforwarder-8.2.6-a6fe1ee8894b-Linux-x86_64.tgz
   ```
3. Extract the tarball and navigate to the `splunk/bin` directory:
   ```bash
   tar -xvzf splunkforwarder-8.2.6-a6fe1ee8894b-Linux-x86_64.tgz
   cd splunk/bin
   ```
4. Start the Splunk forwarder:
   ```bash
   ./splunk start
   ```

---

## Key Components of Splunk

Splunk operates with three primary components:

1. **Search Head**: The interface where users perform searches and interact with data.
2. **Indexers**: Store and index incoming data, making it searchable.
3. **Forwarders**: Collect and forward data to the indexers (Universal Forwarders are lightweight forwarders).

---

## Configuration

### Master Configuration
To configure your master server to listen on port `9997`:

1. Enable Splunk to listen on port `9997`:
   ```bash
   ./splunk enable listen 9997
   ```

### Slave Configuration
To forward data from the slave to the master server:

1. Add the master server’s IP and port to the forwarder configuration:
   ```bash
   ./splunk add forward-server <master-ip>:9997
   ```

---

## Adding Data Sources

You can add a syslog file to the slave using WinSCP, and move it to the `/var/log` directory:

1. Add the syslog file to be monitored:
   ```bash
   ./splunk add monitor /var/log/syslog --index main
   ```

---

## Running Queries in Splunk

To search for data, you typically write a query using the following elements:

- **Index**: Specifies where the data is stored.
- **Host**: The machine sending the data.
- **Source**: The file path where the data originates.
- **Source Type**: The type of data or log.

### Example Query
```spl
index="main" host="hostname" source="/var/log/syslog" sourcetype="syslog"
```

This query will search for logs in the "main" index from a specific host and file path.

---

By following this guide, you’ll have a basic Splunk environment set up, with the ability to forward and search through logs. Stay tuned for more advanced topics!

--- 

Let me know if you need any additional details or modifications!
