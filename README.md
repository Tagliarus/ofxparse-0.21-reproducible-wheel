# ofxparse 0.21 isolated wheel recipe

This small repository is a reproducibility recipe for the upstream `ofxparse` 0.21 source release. It is not an application repository and contains no package source, generated wheel, or credentials.

## Inputs and attribution

- Source distribution: [`ofxparse-0.21.tar.gz`](https://files.pythonhosted.org/packages/45/ae/98a2acfd06d15869c4b1be7fb74849c8a67cf15b65181f1fe879547e7494/ofxparse-0.21.tar.gz)
- Expected source size: `53837` bytes
- Expected source SHA-256: `057ab68d31270dece4d1a47662096aa76341968aaee145ffc711cb44cbd5c4a7`
- Upstream project metadata declares the MIT license. See the [upstream project](https://pypi.org/project/ofxparse/0.21/) and its [source license](https://github.com/jseutter/ofxparse/blob/master/LICENSE).

The workflow downloads the source and build-tool wheels from the official Python package host, verifies each pinned size and SHA-256, and resolves the official Python image to a digest before fetching build inputs. The offline toolchain is pinned to pip `26.3` (`f3b60b20ffaadd4415ed7957212a38ac63fa4c5fa375bd0f4a797777940fad13`) and setuptools `84.2.1` (`ca140b45d5325a9478f880e351484f62fef15f336462e449ca0c03c873d589c2`).

## Build boundary

The recipe performs two builds from the same verified source and digest-pinned image. Package-controlled build code runs only in Docker with networking disabled, a read-only root filesystem and inputs, dropped capabilities, no-new-privileges, and a non-root UID. Each build has its own writable work and output directories. Trusted controls run in the same container before the backend; a failed or unmeasured control stops the build. The two wheel byte streams must match.

`SOURCE_DATE_EPOCH=1783123200` is a fixed recipe policy input; it is not claimed to have been observed in the upstream source. The workflow transfers one exact four-member ZIP through one to four indexed Base64 outputs (at most 65,536 ASCII characters each), with separate chunk count, raw byte size, and SHA-256 outputs. The raw ZIP is capped at 196,608 bytes; the producer and publisher validate canonical ordering, empty trailing slots, per-value limits, and the total UTF-16 job-output budget before any release API write. It does not use Actions artifacts or a package registry. The publisher validates the bundle again and, if explicitly authorized, creates one unpublished draft release with the raw ZIP as its asset. Authenticated release and upload POSTs are pinned to their HTTPS origins and exact paths; redirects are rejected before a request can be replayed with credentials. The workflow never publishes the draft or deletes/retries a partial release.

The only workflow trigger is a push to this repository's `main` branch that changes `.github/workflows/phase83-ofxparse-wheel.yml`. A future one-run draft would use the public run-specific tag `phase83-ofxparse-<run_id>-<attempt>` targeting that run's commit; a draft release does not make its repository, branch, or possible tag metadata private. This recipe does not establish account-wide Actions quota or cost availability. No run, release, or tag is implied by the presence of these files.
