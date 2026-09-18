# Troubleshooting: chip problems, symptom by symptom

Reading, writing and verification problems with the chip itself, ordered from
the most frequent down. Start at your symptom and work through the checks in
order.

This page stops at the chip. A printer that does not show up in Tiger Studio,
or a slot that does not update, is a printer-link question: the
[FAQ](../faq/README.md) has the first two checks, and the
[per-vendor pages](../compatibility/README.md) the rest.

## The chip is not detected

| Check | Detail |
|---|---|
| NFC on, antenna found? | Make sure NFC is enabled, then find your phone's antenna sweet spot — a chip reads best flat against it ([FAQ](../faq/README.md)). Sweep slowly rather than holding one spot. |
| Something in between? | A thick case, or the metal plate of a magnetic mount, sits exactly where the field has to pass. Take it off for the read. |
| Try the other chip | A spool carries two chips, and each backs the other up ([why two chips](../concepts/tigertag-chip.md)). If the second one reads, the first is damaged — a folded or torn sticker has a cut antenna. The surviving chip still identifies the spool; to get a **pair** back, both chips are rewritten together in one session, never a replacement on its own ([when two chips read as two spools](./twin-tag-pair.md)). |
| Another reader? | Any NFC phone, an ACR122U on a computer, a TigerScale or a [TigerSpool](../products/tigerspool.md): if the chip reads on one of them, the chip is fine and the problem is on the reader side. |

## The chip reads as empty

Not a fault: chips sold on their own ship **blank**, logo or no logo
([the TigerTag chip](../concepts/tigertag-chip.md)). Tiger NFC Connect reads
it, sees that it is empty and offers to create a filament for it — that is
step 2 of [your first smart spool](../tutorials/first-smart-spool.md).

## The chip reads, but the data is wrong

The chip was written with the wrong values, or it came off another spool.
Chips are **never write-locked**: re-encode it
([the TigerTag chip](../concepts/tigertag-chip.md)) — a tap in Tiger NFC
Connect, or Tiger Studio's guided, UID-checked write on a desktop reader.

A factory chip you rewrote by accident is the one case with a better fix: if
you had backed it up in Tiger Studio, restore it. The backup is bound to that
chip's UID and puts it back exactly as it was, signature included
([backing up a chip](../products/tigertag-plus.md)).

## One spool, two entries

Each chip reads, and each opens its own spool: the two chips were almost
certainly never a pair. The one test that settles it, and the fix, are in
[when a spool's two chips read as two spools](./twin-tag-pair.md).

## The brand or material shows as unknown, or under the wrong name

The chip carries **IDs**, not names; the app resolves them against its copy of
the shared reference database
([universal filament identity](../concepts/universal-filament-identity.md)).
A brand or material added recently can be on a chip before your copy of the
tables knows it — the chip is right, the table is behind. Tiger Studio ships
the tables bundled and refreshes them from the CDN; a TigerScale refreshes its
own copy at most once a day
([inventory & cloud sync](../concepts/inventory-and-cloud-sync.md)). Let the
app refresh, or update it, then re-scan.

> **Note:** a stale table changes how a chip is *displayed*, never whether a
> TigerTag+ Certified signature *verifies* — see below.

## The phone reads it, but the printer does nothing

Expected. No printer reads a TigerTag chip by itself today — the one exception
is the Snapmaker U1 running the community
[extended firmware](../compatibility/snapmaker.md). Everywhere else the chip's
data reaches the printer **through Tiger Studio**, for the six integrated
brands: that is the [smartphone bridge](../philosophy/smartphone-bridge.md).
Check where your machine stands in the
[compatibility matrix](../compatibility/README.md).

## TigerTag+ Certified: verification fails

Verification checks the signature on the chip against **published public
keys** — free, offline, no account, and no reference database involved
([TigerTag+](../products/tigertag-plus.md)). No network problem can make it
fail. A failed check has two causes, and both are the trust model doing its
job:

| Cause | What it means |
|---|---|
| The chip was rewritten after it was signed | The signature no longer matches the data. The chip stays fully readable — it is simply no longer certified. If you backed it up in Tiger Studio, restore it, signature included. |
| The data was copied onto another chip | The signature is bound to the original chip's UID. A clone fails, on the customer's own phone — by design. |

## A write fails, or stops midway

- **Two taps, and hold.** The write is the *second* tap: tap **Make**, then
  hold the phone against the chip until the app confirms
  ([your first smart spool](../tutorials/first-smart-spool.md)). Pull away
  early and the write does not complete.
- **A half-written chip is not lost.** Chips are never write-locked: scan it
  again and re-encode it from the start.
- **The chip already holds something else** — an old TigerTag, another
  protocol, a plain NDEF tag? It is written over the same way. To start clean,
  erase it first from Tiger NFC Connect or Tiger Studio
  ([the TigerTag chip](../concepts/tigertag-chip.md)).
- **Writing a pair?** In *Dual NFC*, the chip that opened the creation is the
  one the app expects as 1/2 — start with the chip you scanned
  ([tutorial](../tutorials/first-smart-spool.md)).
- **On a desktop reader**, Tiger Studio's write is UID-checked
  ([TigerPOD](../products/tigerpod.md)): keep the chip it scanned on the
  reader until it finishes.

## Still stuck?

Ask on the [Discord](https://discord.gg/3Qv5TSqnJH), or open an issue on the
app's own repository ([repository map](../developers/repositories.md)). A
precise report is a contribution in itself
([support the project](../support.md)) — it needs five things:

1. the chip — NTAG213 / 215 / 216, official or generic, or a factory-tagged
   spool and its brand;
2. the reader — phone model, ACR122U / TigerPOD, TigerScale;
3. the app and its version;
4. what you did, in order;
5. what the app showed, word for word, or a screenshot.

---

**▲ [Documentation index](../../README.md)** · **Related:** [The TigerTag chip](../concepts/tigertag-chip.md), [TigerTag+](../products/tigertag-plus.md), [Compatibility](../compatibility/README.md), [FAQ](../faq/README.md)
