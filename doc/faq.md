# Frequently Asked Questions

## How Is Network Data Generated in DraCor?

We consider the TEI encoding underlying the network data provided by DraCor to be a baseline annotation. Researchers are not limited to this particular approach to deriving character-network data.

Network data in DraCor is based on a specific formalisation of character co-occurrence. Characters are connected when they are encoded as occurring within the same structural unit of a play, typically an act or scene. The primary data source is the `@who` attribute of `<sp>` elements in the TEI files. This usually identifies speaking characters, but non-speaking characters may also be included if silent actions are encoded using `<sp>` and `@who` (see the examples below). Co-occurrence data is calculated automatically and made available through the DraCor API. It is also visualised in the Network tab for each play in the DraCor frontend.
    
Before using this network data for research, we recommend consulting the underlying TEI encoding to determine whether this formalisation is appropriate for the research question at hand. For reproducible analyses, it may also be useful to consult the code that extracts the characters occurring in each segment ([`metrics.xqm`](https://github.com/dracor-org/dracor-api/blob/311d5a5223d751939281be688ea10e15b2632c3c/modules/metrics.xqm#L91-L114)). The resulting data is then passed to the metrics service, which constructs the co-occurrence graph ([`main.py`](https://github.com/dracor-org/dracor-metrics/blob/7c6bd1dead2026d2e458438de1f017e190c20d3c/app/main.py#L20-L40)). Further details are provided in the [Encoding for Network Analysis](/doc/odd#section-network-data) section of the DraCor ODD.

Examples illustrating the contingency of this formalisation include the Prince in Kleist’s [“Prinz Friedrich von Homburg” I.2](https://github.com/dracor-org/gerdracor/blob/4bd92225dfa2b6f1e7af246764b2479e39108758/tei/kleist-prinz-friedrich-von-homburg.xml#L560), Gordon in Schiller’s [“Wallensteins Tod” V.11](https://github.com/dracor-org/gerdracor/blob/4bd92225dfa2b6f1e7af246764b2479e39108758/tei/schiller-wallensteins-tod.xml#L8913), and Klara in Hebbel’s [“Maria Magdalene” III.3](https://github.com/dracor-org/gerdracor/blob/4bd92225dfa2b6f1e7af246764b2479e39108758/tei/hebbel-maria-magdalene.xml#L3054). These characters would not appear as network nodes in the respective segments if their silent actions were not encoded with `<sp>` and `@who`. Whether such actions should be represented in this way depends, among other things, on the definition of a speech act adopted in the encoding and analysis.
    
Where a play does not provide a workable scene structure, different strategies can be used to derive co-occurrence network data. One option is to encode configurations. Pfister defines a configuration as follows: “By configuration we mean the section of the dramatis personae that is present on stage at any particular point in the course of the play. A change in the configuration leads to the constitution of a new scene” ([Pfister 1988, p. 171](https://doi.org/10.1017/CBO9780511553998.008)). Although some theatrical traditions have established terms for such units (“Auftritt” in German, “scène” in French), other traditions lack an equivalent term, while in modern theatre such divisions may not be explicitly marked for other reasons (see [Pfister 1988, pp. 236–239](https://doi.org/10.1017/CBO9780511553998.009)). Pfister therefore proposes the term “configuration”.

DraCor contains several examples in which configurations have been explicitly encoded, including Chekhov’s four final plays: [“The Seagull” (“Чайка”)](https://github.com/dracor-org/rusdracor/blob/d50449dbffb85c486e814e792607722d2f875cd7/tei/chekhov-chaika.xml#L184), [“Uncle Vanya” (“Дядя Ваня”)](https://github.com/dracor-org/rusdracor/blob/d50449dbffb85c486e814e792607722d2f875cd7/tei/chekhov-djadja-vanja.xml#L168), [“Three Sisters” (“Три сестры”)](https://github.com/dracor-org/rusdracor/blob/d50449dbffb85c486e814e792607722d2f875cd7/tei/chekhov-tri-sestry.xml#L196), and [“The Cherry Orchard” (“Вишнёвый сад”)](https://github.com/dracor-org/rusdracor/blob/d50449dbffb85c486e814e792607722d2f875cd7/tei/chekhov-vishnevyi-sad.xml#L195).

Depending on the research question and the underlying TEI encoding, alternative extraction routines may be more appropriate. For example, researchers may choose to exclude non-speaking characters or to enrich the TEI files locally by encoding configurations in Pfister’s sense in order to obtain more fine-grained co-occurrence data for a particular analysis. Such approaches depend strongly on both the source material and the way it is encoded, so DraCor should be understood as providing one transparent and reproducible formalisation rather than the only possible one. We are always interested in hearing about alternative approaches, adaptations, and use cases.

## What Is the “Normalised Year” and How Is It Calculated?

We collect three temporal statements for each play, if available:

1. the year(s) of creation (when the play was written),
2. the year of first printing and
3. the year of first performance.

To facilitate the chronological sorting of plays, for example in corpus overviews or diagrams, we also calculate a “Normalised Year”. This is usually the earlier of the year of first printing and the year of first performance.

However, if a work was first printed or performed more than 10 years after its creation, the year of creation is used as the “Normalised Year”. If the creation date is given as a range, the final year of that range is used. The reasoning behind this is that, for a simple chronological classification, the context of origin is important, for example when describing literary evolution.

For example, Goethe’s [“Urfaust”](https://dracor.org/id/ger000539) was written between 1772 and 1775, but not printed until 1887 and first performed only in 1918. The “Normalised Year” for this play is therefore 1775.

As DraCor is an open-source project, you can inspect the underlying XQuery algorithm [here](https://github.com/dracor-org/dracor-api/blob/88c5d2951d15fa6c6e1d8790d9b514a5e8df65bb/modules/util.xqm#L383).

Please note that you do not have to use our “Normalised Year”; it is provided as a convenient basis for chronological overviews and charts. All other temporal information on the creation, first printing and first performance of a play remains available both in the TEI documents and via the DraCor API.

## What Happens to API Requests Without Version Prefixes?

On 1 December 2023, we published the first stable version of the DraCor API (1.0.0). With this release, we introduced a version prefix into our endpoint URLs (e.g., `https://dracor.org/api/v1/info`). This allows us to run the stable DraCor API side by side with the legacy pre-release version available under the `/v0` prefix (e.g., `https://dracor.org/api/v0/info`).

Requests using the old-style URLs without a version prefix are redirected to the legacy version. For instance, `https://dracor.org/api/info` now redirects to `https://dracor.org/api/v0/info`. Therefore, if you have not adjusted your scripts, they should continue to work as long as redirects are followed.

If this is not the case, you may simply add `/v0` to your API URLs. For example, if you have used the API base URL `https://dracor.org/api`, change it to `https://dracor.org/api/v0`.

Of course, we would appreciate it if you switched to `https://dracor.org/api/v1` sooner rather than later. This would, however, most likely require further changes to your scripts. See the [release notes](https://github.com/dracor-org/dracor-api/releases/tag/v1.0.0) for details of what changed and where you may need to make adjustments.

API version 0.x remains available for backwards compatibility, but may be phased out in the future to avoid maintaining multiple versions of the API indefinitely. We therefore recommend using the current stable API for new applications.

Please also see our blog post, [“Streamlining the DraCor API”](https://weltliteratur.net/streamlining-the-dracor-api/).

## How Should I Cite DraCor in My Research?  

If you would like to cite DraCor in general, please use the following reference:

- Fischer, Frank, et al. (2019). Programmable Corpora: Introducing DraCor, an Infrastructure for the Research on European Drama. In Proceedings of DH2019: “Complexities”, Utrecht University, doi:[10.5281/zenodo.4284002](https://doi.org/10.5281/zenodo.4284002).  

If you use individual corpora in your research, you may cite them via Git commits to improve reproducibility. For details, see:

- Börner, Ingo; Trilcke, Peer (2024). D7.3 On Versioning Living and Programmable Corpora. (Executable) Report and Prototypes for Reproducible Research (v1.0.0). Zenodo, doi:[10.5281/zenodo.11081934](https://doi.org/10.5281/zenodo.11081934), pp. 6–12.  
