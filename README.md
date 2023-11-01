# AvailMonitoring
## 1.Install Prometheus
```
sudo apt-get install -y prometheus prometheus-node-exporter
```
**Create prometheus.yml file**
```
cat > $HOME/prometheus.yml << EOF
global:
  scrape_interval: 15s
  evaluation_interval: 15s

rule_files:
  # - "first.rules"
  # - "second.rules"

scrape_configs:
  - job_name: "prometheus"
    scrape_interval: 5s
    static_configs:
      - targets: ["localhost:9090"]
  - job_name: "avail_node"
    scrape_interval: 5s
    static_configs:
      - targets: ["localhost:9615"]
  - job_name: node
    static_configs:
      - targets: ['localhost:9100']
EOF
```
**Move prometheus.yml to the correct location**
```
sudo mv $HOME/prometheus.yml /etc/prometheus/prometheus.yml
```
**Update the file permissions**
```
sudo chmod 644 /etc/prometheus/prometheus.yml
```
**Ensure Prometheus starts automatically**
```
sudo systemctl enable prometheus.service prometheus-node-exporter.service
```
**Restart Prometheus to activate latest settings**
```
sudo systemctl restart prometheus.service prometheus-node-exporter.service
```
**Check the status, ensure Prometheus has started without errors**
```
sudo systemctl status prometheus.service prometheus-node-exporter.service
```
## 2.Install Grafana
```
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
echo "deb https://packages.grafana.com/oss/deb stable main" > grafana.list
sudo mv grafana.list /etc/apt/sources.list.d/grafana.list
sudo apt-get update && sudo apt-get install -y grafana
```
**Ensure Grafana starts automatically**
```
sudo systemctl enable grafana-server.service
```
**Start Grafana**
```
sudo systemctl start grafana-server.service
```
**Check the status, ensure Grafana has started without errors**
```
sudo systemctl status grafana-server.service
```
## 3.Setup Grafana Dashboard
```
sudo ufw allow 3000/tcp
```
**Ensure port 3000 is open, example of adding to ubuntu firewall**
In your browser navigate to ``http://<your validators ip address>:3000``. The default login username and password is admin/admin

![image](https://github.com/Alping0/AvailMonitoring/assets/105454859/8818e40b-6132-4424-8150-84dd444d268b)

You will be asked to reset your password, please write it down or remember the password as you will need it for the next login.

You will need to create a datasource. Navigate to **Home->Connections->Data sources**

![image](https://github.com/Alping0/AvailMonitoring/assets/105454859/502ad066-4850-4121-84b2-4ab5d972a47a)

Click on **Add data source**

![image](https://github.com/Alping0/AvailMonitoring/assets/105454859/044781a7-27a0-4f74-a827-ee5a38f28f08)

Click on **Prometheus**

![image](https://github.com/Alping0/AvailMonitoring/assets/105454859/6e9d66f6-5520-4e7f-bdca-ecf775c029f8)

Set URL to "localhost:9090", then test and save the connection

![image](https://github.com/Alping0/AvailMonitoring/assets/105454859/ccfc8991-1d42-4261-bfdd-0f2194c97f4f)

Navigate back to your home page, on the top right in the menu select **Import dashboard**

![image](https://github.com/Alping0/AvailMonitoring/assets/105454859/b4a0478b-dbb1-4267-bd98-7836ddbe3823)

Import the Avail Node Metrics file:
https://github.com/availproject/availproject.github.io/blob/6ff2c1862ede87225a1b6ee296ea5762f56f4042/static/grafana/Avail-Node-Metrics.json
**Download that file to your computer.**

![image](https://github.com/Alping0/AvailMonitoring/assets/105454859/36c1fc59-38f7-4d89-8808-4a7c6aa8317d)

You will have a new dashboard that opens and that you can use to monitor your node

![image](https://github.com/Alping0/AvailMonitoring/assets/105454859/0485ddbe-4e22-46b8-b7ae-5e87fb5e109e)








