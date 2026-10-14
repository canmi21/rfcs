---
name: "WAL-G Deletes Every Version"
title: "Whether WAL-G is told B2 keeps versions, so its retention deletes each version by id and not only the name"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

B2 keeps every version under its S3 API with S3 Versioning off, and a delete by name is a hide
marker; the bytes stay, billed, until the bucket's lifecycle rule takes them about a day later.
Our own deletes from the artifact bucket now delete each version by id -- infra's
`spec/architecture/host.md`, "A delete from the artifact bucket takes every version with it" -- but
WAL-G's tiered retention, `delete before` in `apps/data/database/src/keeper.rs`, and each
`backup-mark` overwrite of `metadata.json`, are WAL-G's and cannot be wrapped.

**Read in WAL-G v3.0.9, the version pinned, on 2026-10-14:** `delete before` reaches
`DeleteObjectsWhere` (`internal/delete_handler.go`), which sets `SetShowAllVersions(true)` "if folder
has uncurrent versions we need to clean them as well", and the s3 folder's `DeleteObjects` sends each
object's `VersionId`. Whether the listing carries version ids depends on `isVersioningEnabled`
(`pkg/storages/s3/folder.go`): `S3_ENABLE_VERSIONING=enabled` says yes, `disabled` no, and unset --
as the platform has it -- asks `GetBucketVersioning` and is yes only for `Enabled`. So `enabled`
purges every version and every hide marker before the target, earlier ones included; whether it
already does today rests on what B2 answers `GetBucketVersioning` for a bucket with S3 Versioning
off, which its documentation does not say and one read-only request would. The cost of `enabled`:
every WAL-G listing goes through `ListObjectVersions`, more of B2's class C calls; and the old
versions of a kept backup's `metadata.json` are never before the target, so the lifecycle rule
still takes those.

## Options

**The leaning, not decided:** ask B2 once, read-only, what `GetBucketVersioning` answers for the
backup bucket. If `Enabled`, WAL-G already purges and nothing changes. If not, drill
`S3_ENABLE_VERSIONING=enabled` against a throwaway prefix -- push, mark, `delete before`, then `wal-g
st ls --all-versions`, `backup-list` and a restore -- before setting it beside `WALG_S3_PREFIX` on the
cluster. Until then the lifecycle rule's day is the cost, a few gigabytes for a day.
