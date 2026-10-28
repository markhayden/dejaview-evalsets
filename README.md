# dejaview-evalsets

The images of the public evaluation sets for [dejaview](https://github.com/markhayden/dejaview),
an agentic harness for evaluating image integrity. Everything else about a set lives in the
harness repository under `evalsets/<use-case>/<set>/`: the manifest, one job per item, the
families, and the confirmed answer labels. This repository holds only the image files, laid out
the same way and fetched by SHA-256:

```
<use-case>/<set>/families/<family>/{source.png | ref-<n>.png}   one source or the references of a family
<use-case>/<set>/items/<item>/candidate.png                      the image each item judges
```

`dejaview sets fetch --set <use-case>/<set>` reads the manifest in the harness repository, downloads
each image from this repository at the revision the manifest pins, and refuses any file whose
hash differs from the pin. Nothing here is an answer label: whether a candidate passes or fails is
in the harness repository's `labels.toml`, which is published for `dev` and `heldout` sets and
withheld for `blind` sets until their gate is scored.

## How the images were made

Every item is built by construction so its answer is known before any judge sees it. Sources and
references are generated from a prompt; candidates are made from them by a recorded operation: a
permitted change (a colour grade, a new scene for a reference subject) for pass items, and a
planted defect (a structural transform, an edit that removes or changes a protected feature, a
lookalike) for fail items. The generator for each family is one of FLUX.2 [klein] 4B (Black
Forest Labs, Apache 2.0 weights, via fal), OpenAI gpt-image-2, or Gemini 3.1 Flash Image, mixed
across families so no set carries one vendor's fingerprint. Model, prompt, seed where the API has
one, and every edit instruction are recorded per item in the owner's construction record, and the
owner adjudicates each label from that record before it is published. The method and the vendor
terms are in the harness repository's `docs/adr/0013-set-generation.md`.

Planted defects are cleaner than real failures; every result document that uses these sets says
so.

## Licence

The images in this repository are published under [CC BY 4.0](LICENSE). Attribute
"dejaview-evalsets, Mark Hayden" and link to this repository.
