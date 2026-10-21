# October 2026 TLS trust update

This repository pinned a certifi CA bundle from 2024 that trusts the emSign ECC
Root CA - G3 and AAA Certificate Services paths implicated in the eMudhra
intermediate-key compromise. certifi 2026.10.19 removes these paths. Requests,
HTTPX, and the OpenAI Python SDK use certifi by default. A forged certificate can
impersonate an unrelated API hostname if an attacker can intercept its traffic;
this finding does not establish interception, credential theft, or exploitation.

Rebuild environments from the updated requirements and restart/redeploy after
review. Existing processes, installed packages, published Cowgirl package releases,
and deployed services are not changed by this PR. TLS verification stays enabled.
Custom CA overrides and the operating system trust store need separate review.

Run `python -m unittest discover -s tests -p 'test_tls*.py'` for offline trust checks.
The checks inspect the actual HTTPX context and Requests CA file; they make no API
requests. Before installing dependencies, create a fresh virtual environment on a
maintained Python release, then bootstrap pip 26.2.2 or newer inside it using the
hash-pinned wheel in the PSF advisory below. This upstream pip requires Python
3.10 or newer; retire/recreate older unsupported environments before proceeding.

For distro-managed Python, use the vendor's supported patched pip and CA updates
or the fresh virtual environment path above. Do not bypass PEP 668 with
`--break-system-packages`. Verify vendor backports through the vendor's security
advisory and the effective CA bundle, rather than an upstream version number alone.
App Engine's managed build installer and base-image trust remain provider
responsibilities; an application certifi pin does not update either one.

CI runs the complete `tests` directory with `python -m pytest tests` on Python
3.12, matching the existing dependency lock and lint matrix. The former floating
`3.x` selection chose newer interpreters without compatible wheels for the frozen
scientific dependencies. This change does not skip tests or loosen dependencies;
newer Python versions need a separately validated dependency update.

Sources:
- [Mozilla incident report](https://blog.mozilla.org/security/2026/10/20/protecting-firefox-users-following-the-compromise-of-emudhra-intermediate-certificates/)
- [certifi 2026.10.19](https://github.com/certifi/python-certifi/releases/tag/2026.10.19)
- [Python installer advisory](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576)

`MANIFEST.in` includes `requirements.txt` in source distributions because `setup.py`
reads the dependency list from that file. This preserves all dependency pins,
including certifi, when a wheel is rebuilt from a source distribution. Verify both
package formats before publishing a new release; existing releases are unchanged.

See [externally managed Python environments](https://packaging.python.org/en/latest/specifications/externally-managed-environments/) for the distro/virtual-environment boundary.
