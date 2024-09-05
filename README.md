# FINAL-PROJECT
global:
  smtp_smarthost: 'smtp.gmail.com:587'
  smtp_from: 'halima.alnoobi@gmail.com'
  smtp_auth_username: 'halima.alnoobi@gmail.com'
  smtp_auth_password: 'password'
  smtp_require_tls: true

route:
  group_by: ['alertname']
  group_wait: 5s
  group_interval: 5m
  repeat_interval: 1h
  receiver: 'web.hook'
  routes:
    - match:
        severity: 'critical'
      receiver: 'email-receiver'

receivers:
  - name: 'web.hook'
    webhook_configs:
      - url: 'http://127.0.0.1:5001/'

  - name: 'email-receiver'
    email_configs:
      - to: 'omarmukhayer99@gmail.com'
        send_resolved: true
inhibit_rules:
  - source_match:
      severity: 'critical'
    target_match:
      severity: 'warning'
    equal: ['alertname', 'dev', 'instance']
