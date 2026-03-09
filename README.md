# APC AP8930 Switched PDU Driver for Control4

A Control4 driver for the APC AP8930 Switched Rack PDU, providing full control of all 24 outlets via Telnet CLI.

![Control4](https://img.shields.io/badge/Control4-OS%203.x-blue)
![Outlets](https://img.shields.io/badge/Outlets-24-green)
![Protocol](https://img.shields.io/badge/Protocol-Telnet-orange)

## Features

- **24 Individually Controllable Outlets** - Each outlet appears as a relay binding in Control4
- **Real-Time Power Monitoring** - View current (Amps), voltage, power (kW), and energy (kWh)
- **Outlet Status Display** - See ON/OFF state of each outlet in the Properties panel
- **Outlet Naming** - Retrieves and displays custom outlet names configured on the PDU
- **Device Information** - Shows PDU name, model, location, firmware version, and uptime
- **Automatic Reconnection** - Reconnects automatically if connection is lost
- **Event Triggers** - Fire Control4 events on connect/disconnect

## Supported Hardware

| Model | Description | Outlets |
|-------|-------------|---------|
| AP8930 | Switched Rack PDU, 120V, 20A, (24) 5-20R | 24 |
| AP89xx Series | Other APC Switched Rack PDUs with Telnet CLI | Varies |

## Installation

1. Download `APC_AP8930_PDU.c4z`
2. Open Composer Pro
3. Navigate to **Items → Drivers → Add Driver**
4. Browse to and select the `.c4z` file
5. Add the driver to your project

## Configuration

### Properties

| Property | Description | Default |
|----------|-------------|---------|
| IP Address | IP address of your APC PDU | (empty) |
| Port | Telnet port | 23 |
| Username | Login username | apc |
| Password | Login password | apc |
| Polling Interval | Status refresh interval (seconds) | 30 |
| Debug Mode | Enable verbose logging | Off |

### PDU Setup

Ensure Telnet is enabled on your PDU's Network Management Card:

1. Access the PDU web interface at `http://<pdu-ip-address>`
2. Log in with administrator credentials
3. Navigate to **Configuration → Network → Console**
4. Enable **Telnet** and set the port (default: 23)
5. Save changes

## Usage

### Connections Tab

After adding the driver, go to the **Connections** tab to find 24 relay outputs (Outlet 1-24). Bind each outlet to the device you want to control.

### Actions

| Action | Description |
|--------|-------------|
| Refresh Status | Immediately refresh outlet states and power readings |
| Get Outlet Names | Retrieve custom outlet names from the PDU |
| Get Device Info | Query detailed device information |
| All Outlets On | Turn on all 24 outlets |
| All Outlets Off | Turn off all 24 outlets |

### Commands (for Programming)

| Command | Parameter | Description |
|---------|-----------|-------------|
| Outlet On | OUTLET (1-24) | Turn on the specified outlet |
| Outlet Off | OUTLET (1-24) | Turn off the specified outlet |
| Outlet Reboot | OUTLET (1-24) | Cycle power on the specified outlet |

### Events

| Event | Description |
|-------|-------------|
| Connected | PDU connection established |
| Disconnected | PDU connection lost |

## Properties Display

The driver displays the following information in the Properties panel:

**PDU Information:**
- PDU Name, Model, Location, Contact
- Up Time, Firmware Version

**Power Status:**
- Total Load (Amps)
- Total Power (kW)
- Total Energy (kWh)
- Input Voltage

**Outlet Names:**
- Outlet 1-24 Name (retrieved from PDU)

**Outlet Status:**
- Outlet 1-24 Status (ON/OFF)

## APC CLI Commands

For reference, the driver uses these APC CLI commands:

```
olOn <n|all>         Turn outlet(s) on
olOff <n|all>        Turn outlet(s) off
olReboot <n|all>     Reboot outlet(s)
olStatus all         Get outlet states
olName all           Get outlet names
phReading all        Get phase readings (current/voltage/power/energy)
```

## Troubleshooting

### Driver Status: "Set IP Address"
Enter the IP address of your PDU in the Properties panel.

### Driver Status: "Auth Failed"
Check username and password. Default credentials are usually:
- Username: `apc` / Password: `apc`
- Username: `device` / Password: `apc`

### Driver Status: "Disconnected"
- Verify the PDU is powered on and network connected
- Confirm the IP address is correct
- Ensure Telnet is enabled on the PDU
- Check that port 23 is not blocked by a firewall
- Test connection: `telnet <ip> 23`

### Actions Not Working
Enable Debug Mode to see detailed logging in the Lua tab.

## Version History

| Version | Changes |
|---------|---------|
| 1.0.7 | Added outlet status properties, fixed action commands, suppressed routine logging |
| 1.0.6 | Fixed relay connection structure for Control Outputs |
| 1.0.5 | Initial release with Telnet CLI communication |

## Technical Details

- **Protocol:** Telnet (TCP port 23)
- **Authentication:** Username/password via CLI prompts
- **Polling:** Configurable interval (10-300 seconds)
- **Connection:** Auto-reconnect on disconnect (30 second delay)

---

*This driver communicates via Telnet which transmits data unencrypted. For production environments, ensure your PDU is on a secure, isolated network segment.*
