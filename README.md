# Rulebricks Helm Charts

This repository contains the official Helm charts for deploying Rulebricks and its dependencies.

## Available Charts

- **rulebricks** - The main Rulebricks application chart
- **supabase** - Self-hosted Supabase deployment for database and authentication

## Installation

### Prerequisites

- Kubernetes 1.24+
- Helm 3.10+
- Rulebricks CLI
- A domain name with DNS access
- A Rulebricks license key
## Upgrading

Rulebricks deployments require manual upgrades to ensure controlled rollouts and compatibility testing. There are no automatic update mechanisms built into the charts.

### Using the Rulebricks CLI (Recommended)

```bash
# Check for updates
rulebricks upgrade status

# Upgrade to latest version
rulebricks upgrade

# Upgrade to specific version
rulebricks upgrade --version 1.2.3
```

### Automating Upgrades (Optional)

If you want automatic updates, you can set up your own automation:

**Example using cron:**
```bash
# Add to crontab to check for updates weekly
0 2 * * 0 /usr/local/bin/rulebricks upgrade --yes
```

**Example using Kubernetes CronJob:**
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: rulebricks-updater
  namespace: rulebricks
spec:
  schedule: "0 2 * * 0"  # Weekly on Sunday at 2 AM
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: updater
            image: rulebricks/cli:latest
            command:
            - rulebricks
            - upgrade
            - --yes
          restartPolicy: OnFailure
```

### Using Helm directly
**Note:** Automatic updates are not built into the charts. To automate upgrades, you can:
- Set up a cron job to run `rulebricks upgrade` periodically
- Use a Kubernetes CronJob to check and apply updates
- Integrate with your existing CI/CD pipeline

### Common Issues

1. **Image pull errors**: Verify your Rulebricks license key
2. **Database connection errors**: Check Supabase pod status and logs
3. **Ingress not working**: Ensure DNS is configured

## Support

- Documentation: https://rulebricks.com/docs
- Email: support@rulebricks.com
- GitHub Issues: https://github.com/rulebricks/charts/issues

## License

These charts are provided as part of your Rulebricks license. See your license agreement for details.
