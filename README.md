## 🎯 **Objective**

To deploy and configure **Arkime** as a full network packet capture and analysis system on Ubuntu, enabling scalable indexing, searching, and enrichment of packet captures (PCAPs) through **Elasticsearch** and integrations such as **MaxMind** for geolocation and **AbuseIPDB** for OSINT enrichment.

---

## 🧠 **Skills Learned**

* Hands-on experience in **deploying and configuring Arkime** for enterprise-level network packet capture and analysis.
* Gained proficiency in **Linux system administration**, dependency resolution, and service management using `systemctl`.
* Learned to **integrate Elasticsearch and MaxMind** for data indexing and geolocation enrichment.
* Developed skills in **packet analysis, network traffic investigation, and pcap ingestion** workflows.
* Enhanced understanding of **OSINT enrichment and automation** through the **Cont3xt** integration with tools like AbuseIPDB.

---

## 🛠️ **Tools Used**

* **Arkime** – Open-source full packet capture and analysis system.
* **Ubuntu 22.04 LTS** – Server environment for hosting Arkime.
* **Elasticsearch** – Backend database for indexing and searching network sessions.
* **MaxMind GeoIP** – For IP geolocation enrichment.
* **Cont3xt** – Arkime’s integrated OSINT enrichment tool.
* **AbuseIPDB** – Threat intelligence and IP reputation data source.
* **wget, dpkg, systemctl, nano** – Linux utilities used for installation and configuration.

---

## ⚙️ **Project Implementation Steps**

### **1. Prepare the Environment**

* Spin up an **Ubuntu 22.04 server** with at least **8 GB RAM** and **50 GB disk space**.
* Update and upgrade all system repositories:

  ```bash
  sudo apt-get update && sudo apt-get upgrade -y
  ```

---

### **2. Download and Install Arkime**

* Navigate to the official Arkime website and copy the download link for **Ubuntu 22.04 (AMD)**.
* Run:

  ```bash
  wget <arkime_download_link>
  sudo dpkg -i <arkime_package.deb>
  sudo apt -f install -y
  ```
* Verify installation in the `/opt/arkime` directory.

---

### **3. Configure Arkime**

* Run the configuration script:

  ```bash
  sudo /opt/arkime/bin/Configure
  ```
* When prompted:

  * Select your network interface (e.g., `ens33`)
  * Install Elasticsearch locally for demo use
  * Set an admin password
  * Enable GeoIP file download (requires MaxMind account)

---

### **4. Initialize Elasticsearch**

* Start and initialize the Elasticsearch service:

  ```bash
  sudo systemctl start elasticsearch.service
  sudo /opt/arkime/db/db.pl http://localhost:9200 init
  ```

---

### **5. Create an Admin User**

```bash
sudo /opt/arkime/bin/arkime_add_user.sh <username> "<description>" <password> --admin
```

---

### **6. Start Arkime Services**

Start capture and viewer services:

```bash
sudo systemctl start arkimecapture.service
sudo systemctl start arkimeviewer.service
```

---

### **7. Access Arkime Web Interface**

* Open your browser and navigate to:

  ```
  http://<Arkime_Server_IP>:8005
  ```
* Log in using your admin credentials.
* Explore the **Sessions**, **SPI View**, **Connections**, and **Hunt** tabs to analyze traffic.

---

### **8. Configure MaxMind GeoIP Integration**

* Register for a MaxMind account at [https://www.maxmind.com](https://www.maxmind.com).
* Generate a **license key** under “Manage License Keys.”
* Install GeoIP update utility:

  ```bash
  sudo apt install geoipupdate -y
  ```
* Edit configuration:

  ```bash
  sudo nano /etc/GeoIP.conf
  ```

  Add:

  ```
  AccountID <your_account_id>
  LicenseKey <your_license_key>
  EditionIDs GeoLite2-ASN GeoLite2-Country GeoLite2-City
  ```
* Apply updates and restart Arkime capture:

  ```bash
  sudo geoipupdate
  sudo systemctl restart arkimecapture.service
  ```

---

### **9. Enable and Configure Cont3xt**

* Configure Cont3xt OSINT enrichment service:

  ```bash
  sudo /opt/arkime/bin/Configure --cont3xt
  sudo nano /opt/arkime/etc/cont3xt.ini
  ```

  Update the following:

  ```
  elasticsearch = http://localhost:9200
  passwordSecret = <random_password>
  port = 3218
  ```

* Restart the service:

  ```bash
  sudo systemctl restart arkimecont3xt.service
  ```

* Access Cont3xt:

  ```
  http://<Arkime_Server_IP>:3218
  ```

---

### **10. Integrate AbuseIPDB in Cont3xt**

* Create an **AbuseIPDB** account and generate an **API key**.
* In Cont3xt, navigate to **Settings → Integrations**, add **AbuseIPDB**, and paste your API key.
* Save settings and begin searching IOCs for OSINT enrichment.

---

### ✅ **End Result**

You now have a fully operational **Arkime full-packet capture system** integrated with:

* **Elasticsearch** for session indexing and querying
* **MaxMind** for geolocation enrichment
* **Cont3xt** and **AbuseIPDB** for OSINT intelligence gathering

This setup provides a complete environment for **network forensic analysis**, **incident investigation**, and **SOC-level packet inspection** workflows.

---

### **Screenshots**

---

<div>
    <img src="https://i.imgur.com/lXtA2CW.png" />
</div>

*Ref 1: Arkime Web Interface*

<div>
    <img src="https://i.imgur.com/ozHbCSZ.png" />
</div>

*Ref 2: Cont3xt Web Interface*
