# apigee-opdk-playbook-maintenance-expand-region — Add a Datacenter to an Existing Apigee OPDK Planet

> **A runbook that adds a second (or third) datacenter to a running Apigee Edge Private Cloud planet** — without taking the existing region down. A 51-role dependency graph that joins the new DC's Cassandra ring, stands up cross-DC Postgres replication, re-registers routers, and validates the expanded topology.

> [!NOTE]
> Engineering portfolio note — this project demonstrates multi-datacenter topology choreography with live-planet expansion and pre-flight validation. See the [skills assessment →](SKILLS-ASSESSMENT.md) for the expertise applied.

This is the multi-region expansion counterpart to the [rolling-upgrade runbook](https://github.com/carlosfrias/apigee-opdk-playbook-maintenance-opdk-upgrade). Ansible is the medium; the durable work is the **multi-datacenter topology choreography** — ring merge, cross-DC replication, and pre-flight validation across 9 component groups.

<!-- BEGIN Google Required Disclaimer -->

## Not Google Product Clause

This is not an officially supported Google product.
<!-- END Google Required Disclaimer -->

---

## What the runbook actually does

- **Pre-flight** — `opdk-internal-port-connectivity-validator.yml` validates internal port connectivity across all component groups in the new DC before any install.
- **Provision the new DC** — OS prerequisites, OpenJDK, bootstrap, silent-install configuration for the new region.
- **Join the Cassandra ring** — new DC data nodes rebuild from the established DC (`rebuild dc-<source>`), merging into the existing ring.
- **Stand up Postgres** — new DC master/standby with cross-region replication to the existing analytics datastore.
- **Install + register components** — MS, routers, message processors, Qpid, LDAP, UI in the new DC, registered against the Management Server.
- **Re-register routers** — cross-DC router re-registration so traffic routes to the expanded planet.
- **Validate + capture** — log/config forensic harvest (`opdk-setup-log-files.yml`).

---

## Files

| File | Purpose |
|------|---------|
| `requirements.yml` | The 51-role dependency graph (the architecture) |
| `installation.yml` (via the roles) | The ordered expansion runbook |
| `opdk-internal-port-connectivity-validator.yml` | 9-group pre-flight port validation |
| `installation-rollback.yml` | Reverse the expansion |
| `clean.yml` | Tear down the added DC |
| `opdk-setup-log-files.yml` | Per-group forensic log/config capture |

---

## Usage

```bash
ansible-playbook installation.yml -i your_inventory   # add a DC to the existing planet
ansible-playbook installation-rollback.yml -i your_inventory   # reverse the expansion
```

Bring your own inventory with the existing DC and the new DC's groups (`dc-1`, `dc-2`, …, component groups). Expects the `apigee-opdk-*` role corpus (composed via `requirements.yml`). See [`apigee-edge-opdk`](https://github.com/carlosfrias/apigee-edge-opdk) for the framework.

---

## Provenance

Authored and maintained by **Carlos Frias** during his tenure on Apigee Edge Private Cloud. The multi-region expansion counterpart to the [rolling-upgrade runbook](https://github.com/carlosfrias/apigee-opdk-playbook-maintenance-opdk-upgrade); part of the `apigee-opdk-*` role corpus.

Contributions welcome — see [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

See [LICENSE](./LICENSE).