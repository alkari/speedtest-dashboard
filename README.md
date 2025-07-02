# **Streamlit Internet Speed Dashboard**

This project provides a self-hosted dashboard to monitor internet speed (download, upload, ping) over time. It runs as a systemd service on Debian/Ubuntu Linux, automatically performing tests and persisting history to disk.

## **Features**

* **Automated Speed Tests:** Configured to run every 10 minutes.  
* **Real-time Metrics:** Displays current download, upload, and ping speeds.  
* **Historical Charts:** Visualizes performance trends.  
* **Persistent History:** Saves test results to speedtest\_history.csv.  
* **Systemd Service:** Runs reliably in the background.  
* **Secure Setup:** Uses a dedicated non-privileged user and temporary directories.

## **Prerequisites**

* Linux server (e.g., Ubuntu, Debian)  
* sudo privileges for installation  
* Python 3.x

## **Installation**

The installation is automated via the setup.sh script.

1. **Clone this repo:**  
   git clone https://github.com/alkari/speedtest-dashboard.git,  
   cd speedtest-dashboard

2. **Make setup.sh executable:**  
   chmod \+x setup.sh  
3. **Run the setup script with sudo:**  
   sudo ./setup.sh

## **Operation**

Once setup.sh completes, the dashboard runs in the background.

* Access the Dashboard:  
  Open your browser to:  
  http://\<Your-Server-IP\>:8503

  Replace \<Your-Server-IP\> with your server's IP.  
* **Check Service Status:**  
  sudo systemctl status speedtest-dashboard.service

* **View Logs:**  
  sudo journalctl \-u speedtest-dashboard.service \-f

* **Manage Service:**  
  sudo systemctl stop speedtest-dashboard.service  
  sudo systemctl start speedtest-dashboard.service  
  sudo systemctl restart speedtest-dashboard.service

## **Customization**

* **Test Interval:** Modify TEST\_INTERVAL\_MINUTES in speedtest\_dash.py.  
* **Port:** Change \--server.port in start\_dashboard.sh and speedtest-dashboard.service. Update firewall rules if needed.  
* **History File:** HISTORY\_FILE in speedtest\_dash.py determines the CSV location (/opt/speedtest-dashboard/ by default).

## **Troubleshooting**

* **"Failed to start service"**:  
  * Check sudo journalctl \-u speedtest-dashboard.service \-f for errors.  
  * Verify file placement, content, and permissions.  
  * Ensure Python 3 and python3-venv are installed.  
* **"Permission denied" errors**:  
  * Confirm /opt/speedtest-dashboard and its contents are owned by speedtest-app:speedtest-app.  
  * Ensure ProtectHome=true is enabled in speedtest-dashboard.service and export HOME="/tmp/streamlit\_user\_data" is set in start\_dashboard.sh.  
* **Dashboard not accessible**:  
  * Confirm service is active (running).  
  * Check firewall (e.g., ufw status) for port 8503 (or chosen port).  
  * Verify server IP address.
