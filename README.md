# IUCN Global Ecosystem Typology Ontology Draft

This repository contains an initial OWL/RDF ontology draft based on the **IUCN Global Ecosystem Typology 2.0**. The ontology is intended as an exploratory Semantic Knowledge Graph model for ecosystem classification, ecological drivers, assembly filters, ecological traits, and example ecosystem instances.

This is an independent draft ontology. It is **not** an official IUCN ontology.

## Status

This ontology is a **first draft** and is subject to significant change. It was created quickly as an initial modeling pass for discussion, review, and experimentation. The current structure should be treated as provisional, especially the modeling of ecological filters, traits, sample instances, and any source-derived definitions.

Likely future changes include:

- Refining class names and definitions.
- Revising ecological driver/filter/trait modeling.
- Adding or removing sample ecosystem instances.
- Aligning with external ecosystem, biodiversity, climate, land-cover, or conservation ontologies.
- Adding mappings to other classification systems.
- Reviewing licensing and attribution details before broader reuse.

## Source document

The primary source document is:

Keith, D. A., Ferrer-Paris, J. R., Nicholson, E., & Kingsford, R. T. (eds.) (2020). *The IUCN Global Ecosystem Typology 2.0: Descriptive profiles for biomes and ecosystem functional groups*. Gland, Switzerland: IUCN.

- DOI: <https://doi.org/10.2305/IUCN.CH.2020.13.en>
- IUCN Library page: <https://portals.iucn.org/library/node/49250>
- Official PDF: <https://portals.iucn.org/library/sites/library/files/documents/2020-037-En.pdf>
- IUCN conservation tool page: <https://iucn.org/resources/conservation-tool/iucn-global-ecosystem-typology>
- Interactive Global Ecosystem Typology site: <https://global-ecosystems.org/>

## Overview of the IUCN Global Ecosystem Typology

The IUCN Global Ecosystem Typology is a hierarchical classification system for ecosystems. Its central idea is to integrate two dimensions of ecosystem classification:

1. **Functional similarity** — especially in the upper levels of the hierarchy.
2. **Compositional similarity** — especially in the lower levels of the hierarchy.

The upper levels classify ecosystems by convergent ecological functions, drivers, dependencies, and traits. The lower levels are intended to represent more detailed compositional variation among ecosystems that may be functionally similar but have different biota.

The typology has six hierarchical levels:

| Level | IUCN term | Role in the typology |
|---:|---|---|
| 1 | Realm | Major components of the biosphere, such as terrestrial, freshwater, marine, subterranean, and atmospheric systems. |
| 2 | Functional Biome | Components of realms united by major ecological drivers regulating ecosystem functions. |
| 3 | Ecosystem Functional Group | Related ecosystems within a biome that share ecological drivers and convergent biotic traits. |
| 4 | Biogeographic Ecotype | Top-down ecoregional expressions of Ecosystem Functional Groups. |
| 5 | Global Ecosystem Type | Bottom-up groupings of ecosystems based on compositional similarity and assigned to Ecosystem Functional Groups. |
| 6 | Subglobal Ecosystem Type | Local, national, or subnational ecosystem classification units. |

A key modeling nuance is that Levels 4 and 5 are alternative pathways beneath Level 3. Level 5 is not nested inside Level 4.

The IUCN report also emphasizes:

- Core and transitional realms.
- A global hierarchy of 25 functional biomes and 108 Ecosystem Functional Groups in version 2.0.
- Ecological drivers and assembly filters, including resource filters, ambient environmental filters, disturbance regimes, biotic interactions, and anthropogenic filters.
- Ecological traits such as productivity, trophic structure, energy sources, biogenic structure, phenology, and water conservation.
- The use of descriptive profiles, exemplar photographs, conceptual assembly diagrams, and indicative distribution maps.
- The importance of cross-walking global ecosystem classes to local and national classification systems.

## Overview of this ontology draft

The ontology namespace is:

```text
https://www.michaeldebellis.com/iucn/
```

The ontology IRI is:

```text
https://www.michaeldebellis.com/iucn
```

The current draft builds on a simplified upper ontology foundation derived from Big_Bro/SULO/PROV-Lite and adds IUCN-specific ecosystem classes, properties, controlled-value individuals, and sample instances.

### Naming conventions

The current draft uses the following conventions:

- Classes and individuals use `Title_Case_With_Underscores`.
- Object, data, and annotation properties use lowercase names with underscores.
- New entities use IRIs in the ontology namespace, for example:

```text
https://www.michaeldebellis.com/iucn/Tropical_Subtropical_Forests_Biome
```

### Modeling approach

The ontology currently treats the IUCN hierarchy as a **domain model of ecosystem classes**, not as a metadata model of classification records.

For example, `Terrestrial_Realm`, `Tropical_Subtropical_Forests_Biome`, and `Tropical_Subtropical_Lowland_Rainforests` are modeled as OWL classes of ecosystems. They are not modeled as instances of a separate `ClassificationUnit` or `InformationObject` class.

This decision was made to keep the ontology focused on the domain itself: ecosystems, their hierarchy, their drivers, their traits, and examples. A later version may add a separate metadata layer for IUCN profile documents, map layers, versions, codes, and classification records, but that is intentionally out of scope for this initial draft.

## Current contents

### Core ecosystem hierarchy

The ontology includes a top-level `Ecosystem` class and the six IUCN hierarchy level classes:

- `Realm`
- `Functional_Biome`
- `Ecosystem_Functional_Group`
- `Biogeographic_Ecotype`
- `Global_Ecosystem_Type`
- `Subglobal_Ecosystem_Type`

### Core and transitional realms

The ontology includes the five core realms:

- `Terrestrial_Realm`
- `Subterranean_Realm`
- `Freshwater_Realm`
- `Marine_Realm`
- `Atmospheric_Realm`

It also includes transitional realm classes, such as:

- `Freshwater_Terrestrial_Transitional_Realm`
- `Freshwater_Marine_Transitional_Realm`
- `Marine_Terrestrial_Transitional_Realm`
- `Subterranean_Freshwater_Transitional_Realm`
- `Subterranean_Marine_Transitional_Realm`
- `Marine_Freshwater_Terrestrial_Transitional_Realm`

The core realms are not declared disjoint, because the IUCN typology explicitly recognizes continuous transitions and overlaps among realms.

### Functional biomes

The ontology includes the 25 Level 2 functional biome classes from the IUCN Global Ecosystem Typology 2.0, including examples such as:

- `Tropical_Subtropical_Forests_Biome` (`T1`)
- `Temperate_Boreal_Forests_And_Woodlands_Biome` (`T2`)
- `Savannas_And_Grasslands_Biome` (`T4`)
- `Subterranean_Lithic_Biome` (`S1`)
- `Palustrine_Wetlands_Biome` (`TF1`)
- `Rivers_And_Streams_Biome` (`F1`)
- `Marine_Shelf_Biome` (`M1`)
- `Deep_Sea_Floors_Biome` (`M3`)
- `Brackish_Tidal_Biome` (`MFT1`)

### Ecosystem Functional Groups

The ontology includes the 108 Level 3 Ecosystem Functional Group classes from version 2.0 of the IUCN typology, including examples such as:

- `Tropical_Subtropical_Lowland_Rainforests` (`T1.1`)
- `Trophic_Savannas` (`T4.1`)
- `Rice_Paddies` (`F3.3`)
- `Photic_Coral_Reefs` (`M1.3`)
- `Intertidal_Forests_And_Shrublands` (`MFT1.2`)

Some naming inconsistencies in the IUCN report were normalized by preferring the Part II profile titles over compact appendix labels where they differed.

### Ecological drivers, assembly filters, and traits

The ontology includes an initial model of ecosystem characteristics, including:

- `Ecological_Characteristic`
- `Ecological_Driver`
- `Assembly_Filter`
- `Resource_Filter`
- `Ambient_Environmental_Filter`
- `Disturbance_Regime_Filter`
- `Biotic_Interaction_Filter`
- `Anthropogenic_Filter`
- `Ecological_Trait`
- `Ecosystem_Level_Trait`
- `Species_Level_Trait`

Finite controlled values are generally modeled as named individuals rather than string literals. For example:

- `Water_Filter`
- `Nutrient_Filter`
- `Temperature_Filter`
- `Fire_Regime_Filter`
- `Flood_Regime_Filter`
- `Herbivory_And_Predation_Filter`
- `Ecosystem_Engineer_Filter`
- `Structural_Transformation_Filter`
- `Pollution_Filter`
- `Climate_Change_Filter`
- `Productivity_Trait`
- `Trophic_Structure_Trait`
- `Energy_Source_Trait`
- `Biogenic_Structure_Trait`
- `Phenology_Trait`
- `Water_Conservation_Trait`

The current draft includes object properties for linking ecosystems to these drivers, filters, and traits. More detailed OWL restrictions connecting specific EFGs to drivers and traits have been postponed pending review.

### Sample instances

The ontology includes a small number of example ecosystem instances derived from examples in the IUCN document, such as:

- `Daintree_Tropical_Rainforest`
- `Serengeti_Trophic_Savanna`
- `Hai_Duong_Rice_Paddy`
- `Red_Sea_Photic_Coral_Reef`
- `Los_Haitises_Red_Mangrove_Forest`

These instances are intended primarily for testing, visualization, demonstrations, and SPARQL/Gruff examples. They should not be treated as a complete or authoritative dataset.

## Example uses

This ontology draft may be useful for:

- Exploring the IUCN ecosystem hierarchy in OWL tools such as Protégé.
- Creating graph visualizations in AllegroGraph Gruff.
- Supporting SPARQL queries across realms, biomes, EFGs, drivers, traits, and sample instances.
- Testing alignment with other ecosystem, climate, biodiversity, land-cover, habitat, or conservation ontologies.
- Supporting early RAG/knowledge graph experiments over ecosystem concepts.
- Evaluating how ecosystem typologies can support climate and conservation applications.

## Licensing and attribution

This repository contains an independent ontology draft by Michael DeBellis. The intended license for this draft is:

**Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)**  
<https://creativecommons.org/licenses/by-nc/4.0/>

This ontology includes labels, definitions, terminology, and conceptual structure adapted from the IUCN Global Ecosystem Typology 2.0. The IUCN source publication states that reproduction for educational or other non-commercial purposes is authorized without prior written permission when the source is fully acknowledged, and that reproduction for resale or other commercial purposes is prohibited without prior written permission.

Commercial use of this ontology or IUCN-derived content may require permission from IUCN and/or other rights holders.

Reused ontology foundations and vocabularies, including Big_Bro, SULO, PROV, SKOS, and DCTERMS components, remain subject to their respective original licenses and notices. See repository notices or ontology metadata for details.

## Disclaimer

This ontology is an independent exploratory model. It is not endorsed by IUCN and should not be interpreted as an official IUCN representation of the Global Ecosystem Typology.

The ontology is provided for discussion, research, education, and prototype development. It may contain modeling errors, incomplete definitions, provisional alignments, and source-derived wording that should be reviewed before any production or public-facing use.
