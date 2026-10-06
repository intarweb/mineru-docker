# mineru-docker

Greenfield: publishes **`ghcr.io/intarweb/mineru`** — the MinerU API image the `lan-docker/mineru`
stack runs. No upstream to mirror; this repo *is* the source.

**Why the name differs from the image.** It can't be `intarweb/mineru`: GitHub repo names are
case-insensitive, so that resolves to the existing `MinerU` **fork**, which stays as it is for the
patch branch it carries upstream. `IMAGE_NAME=mineru` is set as a repo variable, which is how
`tika-docker` publishes `ghcr.io/intarweb/tika` too.

**`docker-stacks` pulls this; it does not build it.** The compose sets
`image: ghcr.io/intarweb/mineru:latest` with no `build:`.

**The version is pinned on purpose.** `mineru[api]==X.Y.Z` matches what the `lan-docker/mineru`
stack actually runs. It was `-U` for one build and that pulled a major version whose CLI had
renamed `--allow-public-http-client`, which crash-looped the stack on first use — a delivery change
turned into a silent service upgrade. To move versions: edit the pin, push, and update the compose
if the CLI changed. `:latest` here means "the last build of this repo", never "current upstream".
