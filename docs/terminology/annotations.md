# Annotations

mzmine uses a variety of annotation methods. These methods include spectral library matches,
compound database annotations, lipid annotations, and manual annotation.

## Spectral library matches

To annotate compounds by matching experimental **MS2 (or MSn)** spectra against spectral libraries
using
the [Spectral library search](../module_docs/id_spectral_library_search/spectral_library_search.md)
module. mzmine supports several open library formats (see
[here](../module_docs/id_spectral_library_search/spectral_library_search.md#downloads-for-open-spectral-libraries))
and also features a [workflow](../workflows/librarygeneration/library_generation.md) to build your
own spectral libraries.

## Compound database annotations

The compound database annotation matches experimental **MS1** data (m/z, RT, CCS, RI) against a
library using
the [Local compound database search](../module_docs/id_prec_local_cmpd_db/local-cmpd-db-search.md)
module. This library may be downloaded or generated in-house.

## Lipid annotations

mzmine's lipid annotations are a rule-based fragmentation framework to confidently annotate lipids
based on **MS2** and/or **MS1** data using
the [Lipid annotation](../module_docs/id_lipid_annotation/lipid-annotation.md) module. You can also
define custom lipids and create your own annotation rules.

## Manual annotation

To annotate compounds manually use the the context menu in the feature table (right click on a row)
and select "Identification" → "Annotate manually". The result is stored as a compound database
match.

## Preferred annotation

The preferred annotation is a combination of spectral library matches, compound database
annotations, and lipid annotations. The most confident annotation is chosen according to a set of
community guidelines. mzmine supports ranking of annotations based on
<!-- markdown-link-check-disable-next-line -->
the [MSI](https://link.springer.com/article/10.1007/s11306-007-0082-2) minimum reporting standards,
<!-- markdown-link-check-disable-next-line -->
the [Schymanski et. al.](https://pubs.acs.org/doi/10.1021/es5002105) scale and
default [mzmine](#default-annotation-ranking-in-mzmine) sorting.

### Ranking according to MSI scale

<!-- markdown-link-check-disable-next-line -->
See: https://link.springer.com/article/10.1007/s11306-007-0082-2

**Level 4 - Unknown compounds**: Not annotated, m/z-only compound database annotation (no RT, RI,
CCS)

**Level 3 - Putatively characterised compound classes**: Compound database annotation backed by
Sirius CSI:FingerId, Lipid annotation based on MS1 data

**Level 2 - Putatively annotated compounds**: Spectral library match to reference libraries, Lipid
annotations with MS2 information, Compound database matches with m/z **and** RT or RI information.

**Level 1 - Identified compounds**: At least two orthogonal metrics confirmed with standards in the
same laboratory. This annotation level is not achieved in mzmine, as it would require
differentiation between lab-internal and lab-external RT and RI measurements.

### Ranking according to Schymanski et. al. scale

<!-- markdown-link-check-disable-next-line -->
See: https://pubs.acs.org/doi/10.1021/es5002105

**Level 5 - Exact mass of interest**: Not annotated, compound database annotation (m/z only, without
isotope matching), MS1 only lipid annotation.

**Level 4 - Unequivocal molecular formula**: Compound database match with isotope score >= 0.75

**Level 3 - Tentative candidates**: Compound database match confirmed with Sirius CSI:FingerId

**Level 2b - by diagnostic fragments**: MS2-based species level lipid annotation

**Level 2a - by a library spectrum match**: Spectral library match to a reference library, molecular
species level lipid annotation.

**Level 1 - Confirmed structure**: Spectral library match to a reference library including
RT or RI matching.

### Default annotation ranking in mzmine

The default sorting algorithm in mzmine is adapted from the Schymanski levels, but specifically
weights a lipid annotation with diagnostic fragments higher than a spectral library match without RT
matching. This is done because this lowers false positive molecular species level annotations and
prefers species level annotations, if diagnostic fragments of multiple molecular species are
present.

We want to point out that we neither prefer or disregard the MSI or Schymanski et. al. scale, but
believe the more granular approach of the Schymanski et. al. scale allows a more intuitive user
experience when sorting the feature table by annotation confidence.

