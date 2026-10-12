# Omusaayi

**Open-source smartphone anaemia screening for pregnant women and adolescent girls in Uganda.**

*Omusaayi* means "blood" in Luganda. A health worker photographs the inner lining of the lower eyelid (palpebral conjunctiva) next to a plain white paper. The app corrects the colour, measures how red the lining is, and estimates haemoglobin. It then flags possible anaemia against WHO cut-offs, suggests a next step and writes a referral note.

**Live app:** https://okelloandrewpeters.github.io/omusaayi/

> ⚠️ **Research prototype.** Omusaayi is a screening aid, not a diagnostic device. The bundled example model is trained on **simulated** data and must not be used on real clients. Positive and borderline results should always be confirmed with a blood haemoglobin test.

## Why

- Anaemia affects about 30% of pregnant women in Uganda, according to a 2021 meta-analysis of 17 studies covering 14,410 women.
- Ugandan antenatal clinics face gaps in haemoglobin testing, especially point-of-care testing, along with stock-outs of test kits and reagents.
- Adolescent girls are largely missing from national anaemia surveys.

A phone camera needs no reagents, so it can screen wherever a health worker is.

## How it works

1. **Colour correction.** A white paper in the frame sets the white balance.
2. **Feature extraction.** Inside the marked lining, glare and shadow pixels are dropped. The app then computes three measures:
   - the erythema index, 100 × log₁₀(R/G)
   - the share of strongly red pixels
   - CIELAB a*
3. **Calibrated estimate.** A ridge regression maps these features to Hb. It is refit on the device from readings paired with a HemoCue result, and its error range comes from leave-one-out validation.
4. **Action.** The estimate is graded against WHO cut-offs for the client's group (pregnant, adolescent girl or non-pregnant woman). Borderline results are sent for a blood test.

## Safety by design

- All image processing runs in the browser. Photos are never uploaded or stored.
- Saved readings contain colour numbers, a client code (no names) and a lab Hb only.
- Nothing is saved without a recorded consent tick.
- Readings stay on the device and can be exported and imported as CSV to pool data across a study team.

## Run it

It is a single static `index.html` with no build step and no server. Open it in any modern browser, or serve the folder:

```bash
python3 -m http.server 8000
```

## Roadmap

- Validation study against HemoCue in Kampala and Wakiso (ethics approval pending)
- Referral and follow-up tracking, with aggregate reports for facilities and districts
- Export to DHIS2 / eCHIS
- Luganda and other local-language interfaces
- Fingernail-bed images as a second signal

## Team

Built by [OapAI Technologies](https://oapaitechnologies.com/) (OapAI Lab), Kampala, with planned collaboration with the Department of Computer Science, Makerere University.

## Licence

[Apache License 2.0](LICENSE)
