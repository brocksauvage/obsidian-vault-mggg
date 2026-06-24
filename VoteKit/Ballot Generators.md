---
tags:
  - votekit
aliases:
  - Ballot Generator
  - BallotGenerator
---

# Ballot Generators

The **`votekit.ballot_generator`** subpackage produces **synthetic [[Preference Profile|preference profiles]]** under various statistical models of voter behavior. This is how you simulate electorates for experiments.

## Two styles of generator

### 1. Standard ("name") generators
Generators that don't model blocs/slates. They sample directly over candidate names.

- **Impartial Culture** — `ic_profile_generator` (every ranking equally likely).
- **Impartial Anonymous Culture** — `iac_profile_generator`.
- **Spatial models** — place candidates and voters in space, derive preferences from distance:
  - `onedim_spacial_profile_generator`
  - `spacial_profile_and_positions_generator`
  - `clustered_spacial_profile_and_positions_generator`
- **Name-based ranking models** driven by a [[Preference Interval]]:
  - `name_pl_profile_generator` (Plackett–Luce)
  - `name_bt_profile_generator` / `..._using_mcmc` (Bradley–Terry)
  - `name_cumulative_profile_generator` ([[Cumulative]] score ballots)

### 2. Bloc / slate generators
Model a polarized electorate of voter **blocs** choosing among candidate **slates**, governed by per-bloc preference intervals and **cohesion** parameters. Configured via **`BlocSlateConfig`**.

- `slate_pl_profile_generator`, `slate_pl_profiles_by_bloc_generator` (slate Plackett–Luce)
- `slate_bt_profile_generator`, `..._using_mcmc` (slate Bradley–Terry)
- `cambridge_profile_generator`, `cambridge_profiles_by_bloc_generator` (Cambridge sampler, drawn from real Cambridge MA data)
- `..._profiles_by_bloc_generator` variants return separate profiles per bloc.

Helper converters: `convert_cohesion_map_to_cohesion_df`, `convert_preference_map_to_preference_df`, `convert_bloc_proportion_map_to_series`.

## Models at a glance

| Model | Idea |
|---|---|
| Impartial (Anonymous) Culture | Uniform random rankings — a null baseline |
| Plackett–Luce (PL) | Sequentially pick candidates ∝ their [[Preference Interval\|interval]] support |
| Bradley–Terry (BT) | Probabilities from pairwise strength comparisons |
| Spatial | Preferences from distance in an ideological space |
| Cambridge | Resample empirical ballot patterns from Cambridge, MA |

> [!note] Deprecated class-based API
> Older capitalized classes (`ImpartialCulture`, `PlackettLuce`/`name_PlackettLuce`, `BradleyTerry`, `CambridgeSampler`, `Spatial`, `AlternatingCrossover`, …) still exist in `deprecated_classes` for backward compatibility. New code should prefer the `*_generator` **functions** above.

## See also
- [[Preference Interval]] — the support distribution these sample from.
- [[Preference Profile]] — the output type.
- [[CVR Loaders]] — the real-data alternative.
