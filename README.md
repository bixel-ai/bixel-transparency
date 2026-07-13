# bixel-transparency

The public commitment log for [Bixel](https://bixel.com)'s evidence chain.

Bixel derives company facts from captured public web pages and anchors the
capture history with hash-chained manifest records whose integrity proofs
are **OpenTimestamps proofs anchored in the Bitcoin blockchain**. This
repository holds, for every chain record (a "meta"):

- `log/<name>.sha256` — the SHA-256 of the meta record's exact bytes
- `proofs/<name>.ots` — the OpenTimestamps proof committing to that hash

Nothing else. No page content, no manifests, no counts — commitments only.

## What this gives you

Custody separation. The hashes live in a git history on infrastructure
Bixel does not control retroactively: once a commitment is logged here,
Bixel could not later rewrite that record without the mismatch being
visible to anyone. Updated automatically after each export run
(Mondays and Thursdays); `.ots` files are re-committed when their pending
calendar attestations upgrade to Bitcoin-attested.

## How to use it

Every fact on a Bixel record can produce a proof bundle from the open
endpoint (`GET https://api.bixel.com/v1/companies/{domain}/facts/{key}/proof`),
and the bundle embeds its meta record verbatim (`anchor.meta_b64`). To check
a bundle against this log:

```
sha256(base64decode(bundle.anchor.meta_b64))
  == contents of log/<bundle.anchor.meta_key basename>.sha256
```

[`bixel-verify`](https://github.com/bixel-ai/bixel-verify) performs the full
four-step verification (content, merkle inclusion, chain, Bitcoin anchor);
this log is the independent copy of the commitments it checks against.

A passing check proves the record existed as-committed — it never proves
Bixel's extraction of a page is correct. The raw bytes travel with proofs
precisely so you can read the source yourself.

## License

[Apache-2.0](LICENSE)
