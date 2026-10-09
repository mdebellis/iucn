# Agents, Capitals, and Influence — First Draft

*8 October 2026*

## Purpose

This is a reviewable first implementation of the conceptual model in: Rölfer, L., Isaac, R., López-Rodríguez, M. D., Martín-López, B., Celliers, L., and Krause, G. (2025). Networks of influence: Linking capitals and agency to understand actors’ roles in sustainability interventions. One Earth 8. [https://doi.org/10.1016/j.oneear.2025.101495](https://doi.org/10.1016/j.oneear.2025.101495)

The domain module imports the supplied `Bro_Pro` ontology. The included `bro_pro.ttl` is a byte-for-byte copy of the upload. It already contains SULO, P-Plan, and a subset of PROV, with no remaining `owl:imports` statements. This draft adds no import of the full PROV ontology.

The module remains separate from IUCN. Its provisional namespace is: `https://www.michaeldebellis.com/agents_capitals_influence/` These identifiers have not been published as web endpoints.

## Open in Protégé

1. Extract the ZIP, keeping the files together and retaining `catalog-v001.xml`.
2. Open `Wetland_Example_V1.ttl` to inspect the example and both imported modules. To inspect just the domain schema and foundation, open `Agents_Capitals_Influence_V1.ttl` instead.
3. Run the reasoner. For example, `Funding_Capacity` starts as an Agency with the `Influencing_Financial_Flows` form and is classified as `Financial_Agency`.

The local catalog maps the domain ontology and `Bro_Pro` ontology IRIs to the included files. A fresh Protégé window avoids accidentally reusing an already loaded ontology with the same IRI from another folder.

For AllegroGraph/Gruff, load `bro_pro.ttl`, `Agents_Capitals_Influence_V1.ttl`, and `Wetland_Example_V1.ttl` into a new repository, using Turtle format. Load all three: ordinary RDF import does not necessarily follow `owl:imports`. The four SELECT queries assume these triples are visible in the query’s default graph. Their expected row counts are in `VALIDATION.txt`.

## Files

| File | Description |
| --- | --- |
| `Agents_Capitals_Influence_V1.ttl` | Domain vocabulary and controlled values. |
| `Wetland_Example_V1.ttl` | Entirely fictional example data. |
| `bro_pro.ttl` | Original supplied foundation, unchanged. |
| `catalog-v001.xml` | Local Protégé import mappings. |
| `ACI_Data_Checks_V1.ttl` | Optional SHACL data-entry profile. |
| `queries/` | Four queries for inspecting the example. |
| `VALIDATION.txt` | Checks actually performed and their limits. |

The SHACL profile is a separate shapes graph. Do not import it into the ontology as though it were domain data. Validate the example with the foundation and domain schema available; the checks accept inferred types.

## Concepts taken from the paper

Seven capital classes follow Figure 1 (p. 3), with paraphrased definitions: `Human_Capital`, `Political_Capital`, `Financial_Capital`, `Physical_Capital`, `Social_Capital`, `Cultural_Capital`, and `Natural_Capital`.

Five agency-form individuals follow Table 1 and the explanation on pp. 5–6:

| Form | Related capital types |
| --- | --- |
| `Allocating_Human_Resources` | Human |
| `Enacting_Political_Relevance` | Political |
| `Influencing_Financial_Flows` | Financial |
| `Providing_Physical_Goods_And_Assets` | Physical |
| `Steering_Social_Ecological_Discourse` | Natural, social, cultural |

Table 1 calls the political form “enacting political influence”; the prose also uses “enacting political relevance.” The latter is the primary label.

Each form also has a corresponding Agency subclass, defined through a `hasAgencyForm` value. This supports both a controlled vocabulary for recording assessments and OWL classification of agency capacities. An assessment’s form does not make the assessment itself a capacity or an activity.

These annotations document the paper’s capital/form mapping; they do not require every instance to identify all listed capital types and do not infer agency or successful influence from possession of a capital resource.

Actor-to-actor and actor-to-process relations follow Figures 2–3. The paper’s positive/negative distinction is recorded as `Enabling`/`Constraining` relative to the specified process or objective. It is not a moral classification of the actor. `Mixed` and `Undetermined` are explicit additions for data entry.

The paper distinguishes formal power from influence without formal authority, while its actor-process diagrams also use influence more broadly. This draft uses `Influence_Assessment` in that broader actor-process sense. Political capital and agency cover the authority-related cases. It does not equate this concept with PROV’s generic provenance notion of influence.

## Main modeling choices to review

### 1. Capital resources, access, and use are separate.

Capital is a broad subclass of `sulo:Object`. It can encompass a material resource or an intangible feature/aspect. The seven types are not declared disjoint: one resource can contribute in several ways. Context-specific resource/aspect individuals should be used when that classification varies. `Capital_Access_Assessment` records an actor’s access at a particular time. `Available` access does not entail ownership, willingness, application, or influence. `Restricted` or unavailable access must be recorded explicitly; an absent statement is not a resource gap.

### 2. Agency is a capacity; exercising it is an activity.

Agency is a `sulo:Capability` borne by an Actor. `Agency_Exercise` is a `prov:Activity` and therefore a `sulo:Process` through `Bro_Pro`. An exercise can apply or withhold capital and need not achieve its intended result. Generic exercises need not follow a plan. Only a `correspondsToStep` link commits an activity to the P-Plan execution pattern.

### 3. Influence edges are qualified assessment records.

`Influence_Assessment` records the actor, target, form, direction, context, source, assessor, optional represented viewpoint, and relevant period. `Reported`, `Observed`, `Hypothesized`, and `Illustrative` distinguish the record’s status. `Observed` is not a claim of causal identification. Differing reports can coexist. A `Hypothesized` record does not assert a successful execution. Definitions and subjective-assessment concerns come from pp. 6–7 and 10; the record structure and status vocabulary are our engineering choices.

### 4. Planned steps and actual processes remain distinct.

`Sustainability_Intervention_Plan` is a `p-plan:Plan`. `Governance_Step` is a `p-plan:Step` and an information object. `Governance_Process` is a `prov:Activity`. Use `p-plan:correspondsToStep` to connect a particular execution to its step. The P-Plan IRI is `isPreceededBy` (with that spelling). Do not replace it with `sulo:isPrecededBy` or `sulo:precedes`: those classify their arguments as actual processes. No plan-to-execution class equivalence is added.

### 5. Natural capital frames the intervention context.

The paper explicitly treats ecosystems and their condition as framing social and cultural agency (p. 6). The example links the wetland to the `Study_Context`. It does not assert that an organization owns or allocates the ecosystem. A later IUCN bridge can relate this resource/context to the appropriate ecosystem and intervention entities without changing this core.

### 6. OWL and SHACL have different jobs.

OWL supplies class relationships, agency-form classification, and P-Plan dependency entailments. SHACL supplies record completeness, accepted timestamp representations, timezone requirements, and interval checks. The new data properties have no OWL datatype ranges. The SHACL profile accepts `xsd:dateTime` and `xsd:dateTimeStamp` with explicit timezones. Assessment periods are `[appliesFrom, appliesUntil)`; `assessedAt` is separate.

## What the fictional example demonstrates

The regional authority supports a restoration cooperative through funding; the cooperative supplies equipment to a coordination process. These two qualified links yield one candidate indirect path. The query returns the two supporting assessments and their individual signs and statuses. It does not multiply signs or infer a transitive causal influence relationship.

Two synthetic interview records assess the community network’s discourse differently: one sees improved engagement and the other sees delay. The assessments remain separately attributable rather than overwriting one another.

Two access records show funding access changing from `Restricted` to `Available` after approval. All dates and events are invented. The monitoring step has no recorded execution, and the associated training hypothesis is prospective.

## What this draft leaves open

No capital totals, influence-strength scores, confidence probabilities, centrality measures, causal rules, or predictive thresholds have been invented. Network paths identify candidates for investigation, not established effects. The paper proposes an analytical approach, not a calibrated predictive model.

For the next iteration, the useful decisions are:

- Which real intervention or case study should anchor the first dataset?
- Which capital observations and outcomes can actually be measured over time?
- Which source/assessment criteria should be used for testing a hypothesis?

The capital representation and qualified-assessment pattern are provisional design choices worth reviewing now, but neither required delaying this draft.

## Additional vocabulary reference

- [P-Plan](https://www.opmw.org/model/p-plan/)
- [PROV-O](https://www.w3.org/TR/prov-o/)

The implementation uses the exact term IRIs present in the uploaded `Bro_Pro`.
