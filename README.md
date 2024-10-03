# ansible-role-harness-ccm-ecg

Install and configure the ECG on a VM for Harness CCM Auto Stopping.

Will also create an autostopping rule for the instance if one dosn't exist already.

## requirements

You will need an active Harness CCM license, and an API key with `autostopping:read`. If you want to create autostopping rules when they dont exist, you will also need `autostopping:create`.

Right now this role only works for EC2 based VMs.

## role variables

```
ecg_version: 1.2.0
# right now only linux x86 is supported
ecg_platform: linux_amd64

# thresholds for determining "activity" on the instance
ecg_metrics:
  # a string regex to match against processes running on the machine
  process:
  # the percent of cpu to classify as active
  cpu:
  # the amount of used memory to classify as active
  memory:

# settings for your harness account
harness_url: app.harness.io
harness_account_id:
harness_platform_api_key:

# settings for new autostopping rules
autostopping_rule_idle_time_mins: 5
autostopping_rule_cloud_connector:
autostopping_dry_run: false

# specify aws, azure or gcp, otherwise we will check in that order
cloud:
```

If both cpu and memory are specified, bother conditions must be met to be considered active.

## dependencies

None

## example inventory

```yaml
all:
  children:
    aws:
      vars:
        autostopping_rule_cloud_connector: awsconnectorid
        cloud: aws
      hosts:
        one:
          ecg_metrics:
            process: python*
    azure:
      vars:
        autostopping_rule_cloud_connector: azureconnectorid
        cloud: azure
      hosts: 
        three:
          ecg_metrics:
            cpu: 50
            memory: 1Mi
    gcp:
      vars:
        autostopping_rule_cloud_connector: gcpconnectorid
        cloud: gcp
      hosts: 
        three:
          ecg_metrics:
            process: python*
            cpu: 50
            memory: 1Mi
```

## example playbook

```
- name: Servers
  hosts: all
  roles:
    - role: rssnyder.harness-ccm-ecg
      vars:
        harness
          account_id: "wlgELJ0TTre5aZhzpt8gVA"
          platform_api_key: "sat.wlgELJ0TTre5aZhzpt8gVA.XXX"
```

# license

Apache-2.0

# author information

Riley Snyder (riley.snyder@harness.io)
