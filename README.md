# AWS CloudWatch Metrics Dashboard

## Overview

This project demonstrates how to create a custom CloudWatch Metrics Dashboard to monitor key metrics of multiple EC2 instances. The dashboard includes visualizations for CPU utilization, memory usage, and disk usage, providing a comprehensive view of the performance and health of your infrastructure.

## Features

- **CPU Utilization Monitoring**: Visualize and monitor the CPU usage of your EC2 instances in real-time.
- **Memory Usage Monitoring**: Track the available and used memory on your EC2 instances.
- **Disk Usage Monitoring**: Monitor the disk space utilization and ensure there is enough free space available.
- **Customizable Dashboard**: Easily add or remove widgets to tailor the dashboard to your specific monitoring needs.
- **Real-time Alerts**: Set up CloudWatch alarms to notify you when metrics exceed predefined thresholds.

## Prerequisites

Before setting up the CloudWatch Metrics Dashboard, ensure you have the following:

- **AWS Account**: You need an active AWS account.
- **EC2 Instances**: At least one EC2 instance running, with CloudWatch Agent installed for detailed memory and disk metrics.
- **IAM Permissions**: Ensure you have the necessary IAM permissions to create CloudWatch Dashboards, Alarms, and access EC2 metrics.

## Setup Instructions

### Step 1: Install and Configure the CloudWatch Agent

1. **Install CloudWatch Agent**: Install the CloudWatch Agent on your EC2 instances to collect detailed metrics (memory and disk usage).
    - For Amazon Linux 2, use:
      ```bash
      sudo yum install amazon-cloudwatch-agent
      ```

2. **Configure the Agent**: Create a configuration file to specify which metrics to collect. Example configuration:
    ```json
    {
      "metrics": {
        "append_dimensions": {
          "InstanceId": "${aws:InstanceId}"
        },
        "aggregation_dimensions": [["InstanceId"]],
        "metrics_collected": {
          "mem": {
            "measurement": [
              "mem_used_percent",
              "mem_available_percent"
            ],
            "metrics_collection_interval": 60
          },
          "disk": {
            "measurement": [
              "disk_used_percent"
            ],
            "resources": [
              "/"
            ],
            "metrics_collection_interval": 60
          }
        }
      }
    }
    ```

3. **Start the Agent**: Start the CloudWatch Agent to begin sending metrics to CloudWatch.
    ```bash
    sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a start
    ```

### Step 2: Create the CloudWatch Dashboard

1. **Navigate to CloudWatch**: In the AWS Management Console, go to the CloudWatch service.

2. **Create Dashboard**: Select "Dashboards" from the sidebar and click "Create dashboard". Name your dashboard.

3. **Add Widgets**: 
   - **CPU Utilization**: Add a "Line" widget, select "EC2" as the data source, and choose the "CPUUtilization" metric.
   - **Memory Usage**: Add another "Line" widget, select "CWAgent" as the data source, and choose the "mem_used_percent" metric.
   - **Disk Usage**: Add a "Line" widget for disk usage, using the "disk_used_percent" metric.

4. **Customize Layout**: Arrange the widgets on the dashboard to your preference.

### Step 3: Set Up Alarms (Optional)

1. **Create Alarm**: Go to the "Alarms" section in CloudWatch and create a new alarm.
2. **Select Metric**: Choose a metric (e.g., CPU utilization) to monitor.
3. **Set Thresholds**: Define thresholds for the alarm (e.g., alert when CPU usage exceeds 80%).
4. **Configure Notifications**: Set up SNS topics to send email or SMS alerts when the alarm is triggered.

## Usage

- **Accessing the Dashboard**: Once set up, you can access the CloudWatch Metrics Dashboard from the CloudWatch console. The dashboard will display real-time metrics for your EC2 instances.
- **Modifying the Dashboard**: You can add, remove, or rearrange widgets as needed to customize the dashboard according to your needs.
- **Monitoring Alarms**: If you've set up alarms, you'll receive notifications when specific thresholds are breached, allowing you to take timely action.

## Troubleshooting

- **No Data Displayed**: Ensure that the CloudWatch Agent is properly installed and configured on your EC2 instances.
- **Missing Metrics**: Verify that the EC2 instances have the necessary IAM role with CloudWatch permissions.
- **Alarm Not Triggering**: Double-check the alarm thresholds and ensure that the metric data is available in CloudWatch.

## Conclusion

This CloudWatch Metrics Dashboard project helps you monitor the health and performance of your EC2 instances in real-time. It provides an intuitive interface to track essential metrics and set up alerts to respond proactively to any issues.
