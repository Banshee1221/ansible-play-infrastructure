# ansible-play-infrastructure
The global SANBI infrastructure playbook

## Executing the playbook
1. Clone the repo
2. Go into the cloned directory: `cd ansible-play-infrastructure`
3. Download the required roles: `ansible-galaxy install -r requirements.yml -p ./roles`

## Adding monitoring rules

### Location of rules
Monitoring rules are found in the following directory:

```
./files/monitoring/prometheus/alert_rules/
```
Each set of alerts has to have a file with a `.rules` extension.

### Creating a rule set

A rule file looks like the following:

```yaml
groups:
- name: default
  rules:
  - alert: InstanceDown
    expr: "up == 0"
    for: 5m
    labels:
        severity: critical
    annotations:
        description: "{{ $labels.instance }} of job {{ $labels.job }} has been down for more than 5 minutes."
        summary: "Instance {{ $labels.instance }} down"
```

- `groups`: Signify that you are starting a group (required, default).
- `name`: The name of the group you are about to define.
- `rules`: The defined rules of above `name` follows here.
- `alert:` The name of the alert rule you are defining.
- `expr`: The prometheus expression to evaluate for the rule.
- `for`: How long the expr must match before the alert is sent.
- `labels`: Metadata to use for sending out alerts. Currently only severity is defined for `warning` and `critical`.
- `annotations`: Starts the block of text information to send to the alertmanager.
- `descripton`: The long description of the issue.
- `summary`: The short summary of the issue.