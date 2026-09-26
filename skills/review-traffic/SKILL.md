---
name: review-traffic
description: Screen a batch of signups, logins, orders, reviews or log lines for anonymized traffic with VPNDetection, keeping each result next to the record it came from. Use when someone shares an export, a CSV, a table or a log file and asks which entries came from VPNs, proxies, Tor or hosting, or asks for help spotting fraud, abuse or account sharing by network.
---

# Review traffic with VPNDetection

1. Find the IP address in each record, and keep whatever identifies the record (user, email, order id, timestamp) beside it. If a record has more than one address, use the client address, not a server or proxy hop, and say which column you used.
2. Look up every distinct address with a single `lookup_ips` call. Read `coverage` first: a category the key's plan does not include was not checked, so never report those records as clean on that category.
3. Join the results back onto the records and sort them into what they are:
   - `is_resproxy`, `is_mobproxy`: residential or mobile proxy. The address looks like a home or phone connection but is resold, which is the strongest signal of the set.
   - `is_vpn`, `is_tor`: a VPN or a Tor exit. Deliberate anonymization, common among ordinary privacy-minded users too.
   - `is_dcproxy`, `is_hosting`: a datacenter proxy or a hosting range. Usually automation, scripts or servers rather than a person at a device.
   - `is_relay`: a public privacy relay such as iCloud Private Relay, used by ordinary people by default. Weigh it lightly.
   - `is_cdn`: a CDN range. Usually a service fetching on someone's behalf.
4. Report a count per category, then a table of the flagged records with the identifying fields, the address, the category and the provider where known. Then call out patterns that only show up across records: one provider or one address behind many accounts, bursts in time, or several accounts sharing an address.
5. Offer the full joined table as CSV if the person wants to act on it.

Keep the conclusions to what the data shows. A flag says the network is anonymized, not that the person is acting in bad faith, so present the evidence and let the person asking make the call unless they ask you to recommend one.
