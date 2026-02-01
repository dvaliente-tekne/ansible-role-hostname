# Hostname Role

Captures the hostname from `/etc/hostname` on the remote host and caches it as a persistent fact for use by other roles.

## How It Works

1. Checks if `cached_hostname` fact already exists
2. If not cached, reads `/etc/hostname` from the remote host
3. Stores as a **cacheable fact** that persists across plays
4. Sets `hostname` variable for convenient access

## Requirements

Fact caching must be enabled in `ansible.cfg`:

```ini
[defaults]
gathering = smart
fact_caching = jsonfile
fact_caching_connection = /tmp/ansible_facts_cache
fact_caching_timeout = 86400
```

## Provided Variables

| Variable | Description |
|----------|-------------|
| `hostname` | The hostname (set each run) |
| `ansible_facts['cached_hostname']` | Cached hostname (persists across plays) |

## Example Playbook

```yaml
- hosts: servers
  roles:
    - ansible-role-hostname
    - other-role-that-needs-hostname

# In other-role-that-needs-hostname/tasks/main.yml:
# - name: Use hostname
#   ansible.builtin.debug:
#     msg: "Host is {{ hostname }}"
```

## Tags

| Tag | Description |
|-----|-------------|
| `hostname` | All tasks in this role |
| `always` | Cache check and variable setting |

## License

MIT-0

## Author

dvaliente
