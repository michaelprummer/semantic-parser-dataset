# SemPAI Dataset

Evaluation dataset for the paper **SemPAI: LLM-Based Parser Generation for Deterministic Conversion of Industrial Asset Data**.

It contains 186 synthetic test cases. Each case is a pair of industrial asset descriptions with a ground truth. The ground truth states which semantic properties a parser should find in each file, and which should survive or be lost when converting one file into the other.

## Layout

```
aas/      48 cases  Asset Administration Shell
anchor/   48 cases  Anchor objects
json/     45 cases  Plain JSON
wot/      45 cases  W3C WoT Thing Descriptions (TD 1.1)
```

Each case is a folder `<format>/<NN>_<scenario>_<name>/` containing `file_a.json`, `file_b.json` and `_ground_truth.json`. In `aas/` and `anchor/`, 33 of the 48 `file_b` files are plain JSON, which makes those cases cross-format tests.

## Scenarios

- **Full overlap:** A and B describe the same properties, and every property should survive conversion.
- **Partial overlap:** one side has properties the other lacks, and these must be reported as losses.
- **Nested hierarchy:** multi-level asset trees, such as site → system → component.
- **Any-tagged:** fields with no standard meaning that should *not* map to a semantic ID.
- **Units:** ten cases per format that check units are preserved and normalised to UN/ECE REC 20 symbols. They cover SI, imperial, prefixes, compound units, aliases, and ambiguous or unknown units.
- **Format-specific:** native AAS and Anchor structures compared with plain JSON (`aas/07–09`, `anchor/10–12`).

## Ground truth

Properties are identified by semantic IDs: `TYPE`, `STATE`, `NAME`, `POS`, `VEL`, `THERM`, `WEIGHT`, `POWER`, `PRESS`, `FREQ`, `RATIO`, `CURR`, `VOLT`, `SIZE`, `FORCE` and `ROT`.

Main fields in `_ground_truth.json`:

- `file_a_expected_semids` / `file_b_expected_semids`: IDs a parser should extract from each file (absent in 32 `json/` cases)
- `shared_semids`, `a_only_semids`, `b_only_semids`: IDs in both files or only in one
- `a_to_b_expected_survivors` / `b_to_a_expected_survivors`: `semid → bool` for each conversion direction
- `a_to_b_expected_losses` / `b_to_a_expected_losses`: properties expected to be lost
- `a_to_b_expected_units` / `b_to_a_expected_units`: expected unit symbols (units cases only)

## License

[CC BY 4.0](LICENSE). If you use this dataset, please cite the paper above.
