# ☁️ EC2 + Dynatrace OneAgent Monitoring Setup

This guide walks you through creating an EC2 instance, installing Dynatrace OneAgent, monitoring host metrics, and setting up dashboards and alerts.

Screenshot one of the dashboard I have created with metrics: CPU Idle Time and CPU usage %.
![Dynatrace Dashboard](/Users/sandeepkavya/275FFCD0-39B6-4EB1-8BC3-8F1672B7ADEF.jpeg)

---

## 📦 Prerequisites

- AWS Account
- Dynatrace SaaS Account
- `.pem` key pair for EC2 SSH access
- Basic familiarity with Terminal

---

## 🧱 1. Launch an EC2 Instance

1. Go to [AWS EC2 Console](https://console.aws.amazon.com/ec2/)
2. Click **Launch Instance**
   - OS: Amazon Linux 2 (recommended)
   - Instance Type: `t2.micro` or `t3.small`
   - Key Pair: Select or create one
   - Network: Default VPC
   - Security Group: Allow SSH (TCP 22) from your IP
3. Click **Launch**

---

## 💻 2. SSH into EC2 from macOS/Linux

```bash
chmod 400 ~/Downloads/my-key.pem
ssh -i ~/Downloads/my-key.pem ec2-user@<EC2_PUBLIC_IP>
```

---

## 🎯 3. Install Dynatrace OneAgent

1. Log in to your Dynatrace console:
   `https://<your-env-id>.live.dynatrace.com`

2. Go to **Deploy Dynatrace** → Select **Linux**

3. Copy the pre-generated install command. Example:

```bash
wget -O Dynatrace-OneAgent.sh "https://<env-id>.live.dynatrace.com/api/v1/deployment/installer/agent/unix/default/latest?Api-Token=XXXX"
sudo /bin/sh Dynatrace-OneAgent.sh --set-monitoring-mode=fullstack --set-app-log-content-access=true
```

---

## 📊 4. View Host in Dynatrace

1. Navigate to **Dynatrace → Hosts**
2. Find your EC2 instance (`ip-172-31-xx-xx.ec2.internal`)
3. View CPU, memory, disk, processes, and logs

---

## 🧠 5. Create a Dashboard

1. Go to **Dashboards** → Create new → Click **Edit**
2. Add tiles using **Data Explorer**
3. Sample metrics to use:
   - `CPU usage %`
   - `Memory used %`
   - `Disk usage %`
   - `Network traffic`
4. Filter by your EC2 host name

---

## 🔔 6. Set Up Alerts

1. Go to **Settings → Anomaly Detection → Infrastructure**
   - Enable CPU/Memory thresholds
2. Go to **Settings → Integration → Problem Notifications**
   - Add email, Slack, or webhook integrations

---

## 🌐 7. (Optional) Connect AWS Account to Dynatrace

1. Go to **Settings → Cloud and virtualization → AWS**
2. Click **Connect AWS account**
3. Use **CloudFormation** (recommended)
4. Grant read-only access to allow full cloud monitoring

---

## 🗺 Architecture Overview

```text
[ AWS EC2 (Amazon Linux) ]
        ↓ SSH
[ OneAgent Installed ]
        ↓ Secure HTTPS
[ Dynatrace SaaS Backend ]
        ↓
[ Smartscape + Dashboards + Alerts ]
```

---

## ✅ Result

You now have:
- Full-stack monitoring of your EC2 instance
- Live dashboards with system metrics
- Alerts for infrastructure anomalies
- (Optional) AWS-wide visibility if connected

---

## 🧰 Next Steps

- Install an application (e.g., Node.js, Apache) to trace service-level data
- Enable log monitoring
- Add custom dashboards or alerts
