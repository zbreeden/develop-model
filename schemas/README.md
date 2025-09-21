# Schemas — FourTwentyGenesis

This folder contains the canonical JSON Schema definitions used to validate seed and signal payloads in the FourTwentyGenesis project. The goal of these schemas is to enforce a stable contract between human-authored seed manifests and downstream consumers (site generation, ingestion pipelines, or broadcast signals).

## Layout & key files
- Each schema is a YAML file (`*.schema.yml`) and follows JSON Schema (draft 2020-12) conventions.
- Common schemas include `tags.schema.yml`, `glossary.schema.yml`, `emoji_palette.schema.yml`, and `latest.schema.yml` (signals).

## Conventions enforced by schemas
- Keys and identifiers
  - `key` or `id` fields should use stable snake_case: `^[a-z0-9_]+$`.
  - `repo` values are lowercase with hyphens and numbers only (when enforced in a schema).
- Dates and timestamps
  - `ts_utc` should be an ISO 8601 UTC timestamp (ending with `Z`).
  - `date` fields are YYYY-MM-DD.
- Origin and links
  - `origin` objects must include `name`, `url`, and `emoji`.
  - `links` objects typically include `readme` and `page` where applicable.

## Patterns in this repo
- Prefer strict schemas and provide normalizers for human-authored seeds (see `scripts/normalize-modules.js`).
- Replace hyphens with underscores for keys when producing canonical artifacts (e.g., `elemental-system` → `elemental_system`).

## Validation tools
You can validate YAML seeds using either Node.js (Ajv 2020) or C# (NJsonSchema). Examples below assume you are in the repository root.

### Node (recommended for quick validation)
- Install dependencies in `FourTwentyGenesis/scripts`:

```bash
cd FourTwentyGenesis/scripts
npm install
```

- Run the validator script (example for tags):

```bash
node validate-tags.js
# or via npm script
npm run validate-tags
```

The Node validators load the YAML schema and the data, compile the schema with Ajv (draft 2020-12), and print either success or a list of errors with instance paths.

### C# (NJsonSchema) — used in polyglot workbook
If you prefer a .NET-based validator (used in `FourTwentyGenesis.dib`), a simple pattern is:

```csharp
var schema = await JsonSchema.FromFileAsync("schemas/latest.schema.yml");
var errors = schema.Validate(JToken.Parse(jsonContent));
```

NJsonSchema handles YAML-to-JSON conversion if the YAML is parsed into a JObject/JToken first. This is useful when working inside the polyglot notebook cells that mix C# with file I/O.

## CI recommendations
- Add a job that runs the scripts in `FourTwentyGenesis/scripts` and fails the build if any validator reports errors.
- For modules and other seeds with normalizers, run the normalizer and compare the generated artifact to the committed canonical file. Fail if they differ.

Example GitHub Actions snippet (conceptual):

```yaml
- name: Validate seeds
  run: |
    cd FourTwentyGenesis/scripts
    npm ci
    npm run normalize-modules
    npm run validate-emoji
    npm run validate-tags
    npm run validate-modules-normalized
```

## Adding or changing schemas
- If a change adds or removes required properties in a public schema, discuss the change and update README or a migration note. Backward-incompatible changes should be versioned.
- Prefer adding optional properties or new schema files for new features rather than editing existing required fields.

## Troubleshooting
- YAML parse errors: quote values with colons or special characters and ensure list item markers (`-`) are present.
- Meta-schema errors in Ajv: use Ajv's 2020 build (`require('ajv/dist/2020')`) to support draft 2020-12.

If you want, I can also add a CI workflow file and a small helper script that runs all validators and emits a summary JSON for dashboards.
