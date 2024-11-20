---
layout: post
title: "Building a Modern Monitoring System: From Metrics to Alerts"
date: 2024-06-15
categories: technology
published: true
---

# Building a Modern Monitoring System

In this post, I'll walk through building a comprehensive monitoring system using modern tools and practices. We'll cover everything from collecting metrics to sending alerts.

## Metric Collection

First, let's set up our Python collector using Prometheus client:

{% highlight python %}
from prometheus_client import start_http_server, Counter, Gauge
import psutil
import time

# Initialize metrics
CPU_USAGE = Gauge('cpu_usage_percent', 'CPU usage in percent')
MEMORY_USAGE = Gauge('memory_usage_percent', 'Memory usage in percent')
REQUEST_COUNT = Counter('request_count_total', 'Total request count')

def collect_metrics():
    while True:
        # Update CPU and memory metrics
        CPU_USAGE.set(psutil.cpu_percent())
        MEMORY_USAGE.set(psutil.virtual_memory().percent)
        time.sleep(5)

if __name__ == '__main__':
    start_http_server(8000)
    collect_metrics()
{% endhighlight %}

## Alert Configuration

Here's our Alertmanager configuration in YAML:

{% highlight yaml %}
global:
  resolve_timeout: 5m
  slack_api_url: 'https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK'

route:
  group_by: ['alertname', 'cluster', 'service']
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 4h
  receiver: 'slack-notifications'

receivers:
- name: 'slack-notifications'
  slack_configs:
  - channel: '#alerts'
    title: "{{ range .Alerts }}{{ .Annotations.summary }}\n{{ end }}"
    text: "{{ range .Alerts }}🔥 Alert: {{ .Annotations.description }}\n{{ end }}"
{% endhighlight %}