# Bybit native bid/ask — independently usable parts

229 daily CSV.gz files, 110,694,329 native records, 1,740,263,138 original compressed CSV bytes; 20 independent ZIPs, each strictly below 94,371,840 bytes.

No resampling, re-download, or changes to daily files or metadata. Every daily file is included exactly once. Each ZIP contains its own daily sidecars, the unchanged source README and complete provenance, a part-specific manifest, and part-SHA256SUMS. No other ZIP is needed to open or use a part. See PART-README.md inside each ZIP for scope. Original source metadata refers to all days; part manifests identify the subset present. Archive dates are not guaranteed UTC event bounds.

All gzip row counts, original checksums, ZIP member CRC/SHA-256 hashes, exact-once coverage, and strict size limits were verified. See part-index.json for dates, records, inventories and checksums. Verify downloads with `sha256sum -c SHA256SUMS`.

| Part | Bytes | Days | Records | Archive dates |
| --- | ---: | ---: | ---: | --- |
| XAUUSDT-bid-ask-part-001-of-020-2026-03-09-to-2026-03-21.zip | 88054630 | 13 | 5892328 | 2026-03-09–2026-03-21 |
| XAUUSDT-bid-ask-part-002-of-020-2026-03-22-to-2026-03-31.zip | 83387699 | 10 | 5272645 | 2026-03-22–2026-03-31 |
| XAUUSDT-bid-ask-part-003-of-020-2026-04-01-to-2026-04-13.zip | 88951608 | 13 | 5816303 | 2026-04-01–2026-04-13 |
| XAUUSDT-bid-ask-part-004-of-020-2026-04-14-to-2026-04-24.zip | 88686853 | 11 | 5886526 | 2026-04-14–2026-04-24 |
| XAUUSDT-bid-ask-part-005-of-020-2026-04-25-to-2026-05-06.zip | 89224778 | 12 | 5873733 | 2026-04-25–2026-05-06 |
| XAUUSDT-bid-ask-part-006-of-020-2026-05-07-to-2026-05-18.zip | 84318387 | 12 | 5467557 | 2026-05-07–2026-05-18 |
| XAUUSDT-bid-ask-part-007-of-020-2026-05-19-to-2026-06-01.zip | 86473094 | 14 | 5476568 | 2026-05-19–2026-06-01 |
| XAUUSDT-bid-ask-part-008-of-020-2026-06-02-to-2026-06-13.zip | 90594507 | 12 | 5643799 | 2026-06-02–2026-06-13 |
| XAUUSDT-bid-ask-part-009-of-020-2026-06-14-to-2026-06-25.zip | 88940202 | 12 | 5433602 | 2026-06-14–2026-06-25 |
| XAUUSDT-bid-ask-part-010-of-020-2026-06-26-to-2026-07-08.zip | 89459531 | 13 | 5461838 | 2026-06-26–2026-07-08 |
| XAUUSDT-bid-ask-part-011-of-020-2026-07-09-to-2026-07-21.zip | 85127706 | 13 | 5231287 | 2026-07-09–2026-07-21 |
| XAUUSDT-bid-ask-part-012-of-020-2026-07-22-to-2026-08-03.zip | 90071471 | 13 | 5561983 | 2026-07-22–2026-08-03 |
| XAUUSDT-bid-ask-part-013-of-020-2026-08-04-to-2026-08-13.zip | 87999774 | 10 | 5421648 | 2026-08-04–2026-08-13 |
| XAUUSDT-bid-ask-part-014-of-020-2026-08-14-to-2026-08-24.zip | 87773462 | 11 | 5456148 | 2026-08-14–2026-08-24 |
| XAUUSDT-bid-ask-part-015-of-020-2026-08-25-to-2026-09-02.zip | 87466476 | 9 | 5382684 | 2026-08-25–2026-09-02 |
| XAUUSDT-bid-ask-part-016-of-020-2026-09-03-to-2026-09-13.zip | 86716691 | 11 | 5505544 | 2026-09-03–2026-09-13 |
| XAUUSDT-bid-ask-part-017-of-020-2026-09-14-to-2026-09-22.zip | 81697977 | 9 | 5175084 | 2026-09-14–2026-09-22 |
| XAUUSDT-bid-ask-part-018-of-020-2026-09-23-to-2026-10-02.zip | 91604950 | 10 | 5825535 | 2026-09-23–2026-10-02 |
| XAUUSDT-bid-ask-part-019-of-020-2026-10-03-to-2026-10-13.zip | 84518424 | 11 | 5489075 | 2026-10-03–2026-10-13 |
| XAUUSDT-bid-ask-part-020-of-020-2026-10-14-to-2026-10-23.zip | 86570116 | 10 | 5420442 | 2026-10-14–2026-10-23 |
