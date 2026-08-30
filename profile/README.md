# CrowdSky

Time-domain astronomy from a crowd of small telescopes.

Thousands of ZWO Seestar smart telescopes are pointed at the sky every clear
night, and almost all of the raw data they produce is discarded after stacking.
CrowdSky collects it, keeps it, and turns it into a time-domain survey.

Hosted at the University of Vienna. Two public services, a shared client, and the
science that comes out of them.

## The services

| | |
|---|---|
| **[CrowdSky](https://github.com/crowdsky-astro/CrowdSky)** | The stacking service. Seestar owners upload raw FITS; CrowdSky returns 15-minute stacks and keeps them for free. Finer cadence is a paid tier. PHP frontend, MariaDB, u:cloud storage, Python stacking worker. → [crowdsky.univie.ac.at](https://crowdsky.univie.ac.at) |
| **[CrowdSci](https://github.com/crowdsky-astro/CrowdSci)** | The target broker. Tells the crowd what to observe and why, with the aim of integrating into the Seestar app itself. Inherits CrowdSky's architecture; adds a Python module framework so scientists can define their own campaigns. → [crowdsci.univie.ac.at](https://crowdsci.univie.ac.at) |

## The shared contract

**[crowdsky-client](https://github.com/crowdsky-astro/crowdsky-client)** —
a deliberately lightweight Python client for the CrowdSky data API: frame
discovery and download, nothing else. It depends on `requests`, `astropy` and
`astropy-healpix`, and pointedly *not* on the stacking engine.

This is the piece that makes a CrowdSci analysis module behave identically on a
laptop and on the production runner. If you are writing science against CrowdSky
data, this is your entry point.
→ [crowdsky-client.readthedocs.io](https://crowdsky-client.readthedocs.io)

## At the telescope

**[crowdsky_bot](https://github.com/crowdsky-astro/crowdsky_bot)** — the
observer-side agent. Runs beside a Seestar (a Raspberry Pi Zero is enough),
drives it via `seestarpy`, and pushes the night's raws to CrowdSky unattended.

**[crowdsky_app](https://github.com/crowdsky-astro/crowdsky_app)** — a Kivy
mobile front-end for the same job, for people who would rather use a phone than
a Pi.

## The science

**[seestar_papers](https://github.com/crowdsky-astro/seestar_papers)** —
papers built on Seestar S50 and S30 Pro data, much of it gathered through the
Seestar Collective citizen-science community. Instrument characterisation,
a globally-distributed supernova campaign, light-pollution filter analysis,
warm-Jupiter transit searches.

**[photometry-adventures-2](https://github.com/crowdsky-astro/photometry-adventures-2)** —
the MW Cam δ-Scuti characterisation work the photometry pipeline was
generalised out of. Private.

## Related, elsewhere

These live on [@astronomyk](https://github.com/astronomyk) rather than here.
They are general-purpose Seestar tooling with their own release cadences and
their own users, and they are useful whether or not you care about CrowdSky:

- **[seestarpy](https://github.com/astronomyk/seestarpy)** — a lightweight Python
  driver for Seestar smart telescopes. Minor version tracks the oldest supported
  firmware major. → [seestarpy.readthedocs.io](https://seestarpy.readthedocs.io)
- **[seestar_photometry](https://github.com/astronomyk/seestar_photometry)** —
  time-domain photometry from Seestar stacks, calibrated onto Gaia DR3 synthetic
  Johnson V.

## How it fits together

```
  Seestar owners
        │  raw FITS
        ▼
   ┌─────────┐   stacked frames    ┌──────────────────┐
   │ CrowdSky │───────────────────▶│ crowdsky-client  │
   └─────────┘                     └──────────────────┘
        ▲                                   │
        │ observing targets                 │ same API, laptop or production
        │                                   ▼
   ┌─────────┐                     ┌──────────────────┐
   │ CrowdSci │                    │  science modules │
   └─────────┘◀───────────────────-│  → seestar_papers│
       campaigns                   └──────────────────┘
```

---

Maintained by [Kieran Leschinski](https://github.com/astronomyk),
Department of Astrophysics, University of Vienna.
