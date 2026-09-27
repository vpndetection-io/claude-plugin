---
name: database-files
description: Look into the VPNDetection IP databases an organization is licensed for - what exists, what is inside one, how big it is, whether a downloaded copy is intact, and why a download failed. Use when someone asks about the downloadable databases, their columns, sample rows, sizes, build dates, formats (CSV or MMDB), checksums, license standing or download history.
---

# VPNDetection databases

1. Start with `list_databases`. Every published family is listed with its `standing`: `licensed` means the organization holds it today, `expired` means the term ended, `unlicensed` means it has never been bought. Never read an unlicensed or expired family as one they hold. A version's `sample_formats` lists the formats of its free evaluation sample, a small file cut from the real database that the person can download from the Databases page at https://app.vpndetection.io/database/datasets, license or not.
2. Each family has a `base` id, which is what a license names, and `versions[].id`, which is what every other database tool takes: pass `vpn_ip_v1`, never `vpn_ip`. If a tool answers `UNKNOWN_DATASET`, you probably passed the base.
3. To describe one, call `database_metadata` with the versioned id. It answers the columns per format with their types, a few sample rows, the row count, the build date and the file size of each format. Show the columns as a table and the sample rows as they came, and use the sizes to say what a transfer will cost before anyone starts one.
4. To check a copy someone already has, call `database_checksum` with the versioned id and the format, then compare against the file's own digest. In a terminal that is `sha256sum <file>` (or `shasum -a 256 <file>` on macOS). A mismatch means the file is truncated, corrupted or from a different build.
5. To explain a download that failed or stopped working, call `list_downloads`. It lists recent attempts newest first, refusals included, each with its `outcome`, its `http_status` and a `sample` flag, which is true for an evaluation sample rather than the database itself. It is a bounded window, so a database missing from it has not necessarily never been downloaded.
6. There is no download tool, on purpose: the files run to several GB, which is not something to pull into a conversation. Point people to the CLI, which verifies the checksum for them:

   ```console
   vpndetection db download vpn_ip_v1
   vpndetection db download cdn_ip_v1 --format mmdb cdn.mmdb
   ```

   or to the client libraries at https://github.com/vpndetection-io, or the API at https://docs.vpndetection.io.
