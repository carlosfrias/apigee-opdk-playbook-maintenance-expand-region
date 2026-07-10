# Skills Assessment — apigee-opdk-playbook-maintenance-expand-region

> **Skill domain:** Multi-datacenter topology choreography — adding a datacenter to a running Apigee OPDK planet without downtime (the OPDK on-prem tier). Part of the broader Apigee platform-operations portfolio; see the [`bap_coe` portfolio hub →](https://github.com/carlosfrias/apigee-hybrid-workspace/blob/master/SKILLS-ASSESSMENT.md) for the cloud-native (Hybrid/K8s) counterpart and the full corpus.

---

## Why this project is notable

- **51-role dependency graph.** `requirements.yml` composes 51 `apigee-opdk-*` roles into one ordered expansion runbook — the composition *is* the architecture.
- **Live-planet expansion.** Adds a DC to a running planet without downtime to the existing region — the hard case (not greenfield).
- **Cross-DC Cassandra ring merge.** Joins the new DC's Cassandra nodes to the existing ring (`cassandra-rebuild` streams from the established DC).
- **Cross-DC Postgres replication.** Stands up the new DC's Postgres master/standby and pairs it across regions for analytics HA.
- **9-group pre-flight validation.** `opdk-internal-port-connectivity-validator.yml` checks all 9 component-group internal port meshes *before* mutating anything — validate-before-trust.
- **Full rollback + clean.** `installation-rollback.yml` reverses the expansion; `clean.yml` tears it down.

---

## Expertise demonstrated

> Ansible is the medium. The engineering evidence lives in the [project README →](README.md). What follows is the skills assessment for the business reader.

- **Multi-datacenter topology choreography** — adding a DC to a live planet: ring merge, cross-DC replication, router re-registration, no-downtime expansion. This is the hard case — not greenfield, but expanding a running distributed system in place.
- **51-role composition architecture** — `requirements.yml` is the architecture; the runbook is the ordered execution of a large dependency graph. The composition *is* the architecture.
- **Cassandra cross-DC ring merge** — new DC data nodes `rebuild` from the established DC. The same `rebuild dc-<source>` primitive from [`apigee-opdk-cassandra-rebuild`](https://github.com/carlosfrias/apigee-opdk-cassandra-rebuild), composed into the expansion runbook.
- **Cross-DC Postgres replication** — analytics HA across regions. New DC master/standby with cross-region replication to the existing datastore.
- **Pre-flight validation discipline** — 9-group internal port validation before mutation. Validate-before-trust as an architectural principle, not an afterthought.
- **Failure-aware rollback** — `installation-rollback.yml` reverses the expansion; `clean.yml` for teardown. The expansion has a rollback path.

---

## How this shows the expertise

This runbook is the multi-region expansion counterpart to the [rolling-upgrade runbook](https://github.com/carlosfrias/apigee-opdk-playbook-maintenance-opdk-upgrade). Where that runbook upgrades a planet in place, this one adds a datacenter to it — also in place, also without downtime.

The clearest single signal: **the 51-role dependency graph is the architecture.** The composition is not "run these tasks in order" — it is "these 51 roles form a dependency graph, and the runbook walks that graph to expand the planet without taking it down." That is systems choreography at datacenter scale, with Ansible as the medium.

The second signal: **pre-flight validation across 9 component groups.** Before mutating anything, the runbook validates that every component group can reach every other component group on the required internal ports. Validate-before-trust is not a nice-to-have — it is the gate that prevents a partial expansion from corrupting the live planet.

---

## Related expertise

| Skill | Repository | Assessment |
|-------|-----------|-----------|
| Rolling upgrade / DR / traffic fencing | [`apigee-opdk-playbook-maintenance-opdk-upgrade`](https://github.com/carlosfrias/apigee-opdk-playbook-maintenance-opdk-upgrade) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-opdk-playbook-maintenance-opdk-upgrade/blob/master/SKILLS-ASSESSMENT.md) ✅ |
| Cassandra cluster rebuild | [`apigee-opdk-cassandra-rebuild`](https://github.com/carlosfrias/apigee-opdk-cassandra-rebuild) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-opdk-cassandra-rebuild/blob/master/SKILLS-ASSESSMENT.md) ✅ |
| Postgres HA / controlled switchover | [`apigee-opdk-setup-postgres-failover`](https://github.com/carlosfrias/apigee-opdk-setup-postgres-failover) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-opdk-setup-postgres-failover/blob/master/SKILLS-ASSESSMENT.md) ✅ |
| Cloud-native (Hybrid/K8s) counterpart | [`apigee-hybrid-workspace`](https://github.com/carlosfrias/apigee-hybrid-workspace) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-hybrid-workspace/blob/master/SKILLS-ASSESSMENT.md) ✅ portfolio hub |

---

## Provenance

Authored and maintained by **Carlos Frias** during his tenure on Apigee Edge Private Cloud. This skills assessment is the companion to the engineering [README →](README.md). For the full engineering detail — the 51-role graph, files, and usage — see the project README.

## License

See [LICENSE](./LICENSE).