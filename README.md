# mineru-docker

Greenfield: publishes **`ghcr.io/intarweb/mineru`** — the MinerU API image the `lan-docker/mineru`
stack runs. No upstream to mirror; this repo *is* the source.

**Why the name differs from the image.** It can't be `intarweb/mineru`: GitHub repo names are
case-insensitive, so that resolves to the existing `MinerU` **fork**, which stays as it is for the
patch branch it carries upstream. `IMAGE_NAME=mineru` is set as a repo variable, which is how
`tika-docker` publishes `ghcr.io/intarweb/tika` too.

**`docker-stacks` pulls this; it does not build it.** The compose sets
`image: ghcr.io/intarweb/mineru:latest` with no `build:`.

**One thing `:latest` does not mean here.** The Dockerfile is `pip install -U "mineru[api]"`, and a
greenfield build's fingerprint is this repo's own commit sha — so the image is rebuilt when *this*
repo is pushed or when a `REBUILD_EPOCH` org-variable bump forces it, never on the clock.
`:latest` therefore reads as "the last greenfield build", **not** "the current upstream MinerU
release". A stale MinerU means nobody has pushed here in a while; bump `REBUILD_EPOCH` to refresh it.
