# ansible-role-harness-ccm-ecg

Install and configure the ECG on a VM for Harness CCM Auto Stopping.

## requirements

You will need an active Harness CCM license, and an API key with `autostopping:read`.

The EC2 instance you are targeting must have an existing autostopping rule.

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
```

If both cpu and memory are specified, bother conditions must be met to be considered active.

## dependencies

None

## example Playbook

```
- name: Servers
  hosts: all
  roles:
    - role: rssnyder.harness-ccm-ecg
      vars:
        harness
          account_id: "wlgELJ0TTre5aZhzpt8gVA"
          platform_api_key: "sat.wlgELJ0TTre5aZhzpt8gVA.XXX"
        ecg_metrics:
          # watch for any processes that start with `python`
          process: python*
          # active is 50% cpu
          cpu: 50
          # and 1G of memory
          memory: 1G
```

# license

Apache-2.0

# author information

Riley Snyder (riley.snyder@harness.io)
