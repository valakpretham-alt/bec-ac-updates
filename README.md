# BEC Connect Accuracy Checker — update feed

This repository exists **only** to host the Firefox update manifest and the
signed release packages for the BEC Connect Accuracy Checker extension. The
extension's source lives in a separate private repository.

It has to be public. Firefox fetches `updates.json` and the `.xpi` it points to
over HTTPS **without credentials**, so a private repo answers `401` and updates
silently stop working for everyone who has the extension installed.

## What's here

| file | purpose |
|---|---|
| `updates.json` | the update manifest Firefox polls |
| release assets | the AMO-signed `.xpi` for each version, attached to a tag `v<version>` |

## Publishing a release

Generated from the private source repo — do not hand-edit `updates.json`:

```
node tools/make-release.js --xpi <signed.xpi> --out ../bec-ac-updates
```

Then create a release here tagged `v<version>`, attach the signed `.xpi`, and
push `updates.json`. The tag and the asset filename must match exactly what the
generator printed, or Firefox will 404 and stop offering updates.

`updates.json` carries a sha256 `update_hash`; Firefox verifies it before
installing, so a corrupted or swapped download is rejected rather than run.

## A note on what "private source" does and doesn't mean

A `.xpi` is a zip of plain JavaScript, and it is served publicly here. Keeping
the source repo private prevents the code being browsed or indexed on GitHub;
it does not make the code secret. Anyone with a release URL can read it.
