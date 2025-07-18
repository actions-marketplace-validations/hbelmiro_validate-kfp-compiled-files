# ✅ Validate KFP Compiled Files

A GitHub Action to validate that your [Kubeflow Pipelines](https://www.kubeflow.org/docs/components/pipelines/) Python scripts (`.py`) are correctly compiled into their corresponding YAML files (`.yaml`).

This helps ensure consistency in CI pipelines by preventing out-of-date compiled pipeline definitions.

---

## 🔧 How It Works

For each entry in a JSON mapping file (`.py` ➝ `.yaml`), this action:

1. Validates the existence of both files.
2. Compiles the Python file using `kfp dsl compile`.
3. Diffs the compiled output with the existing YAML.
4. Fails the workflow if differences are found.

---

## 📦 Inputs

| Name                 | Description                                           | Required |
|----------------------|-------------------------------------------------------|----------|
| `pipelines-map-file` | Path to the JSON file mapping `.py` ➝ `.yaml` files   | ✅ Yes    |
| `requirements-file`  | Path to the `requirements.txt` file for `kfp` install | ✅ Yes    |
| `extra-compile-args` | Extra arguments to pass to `kfp dsl compile` (optional) | ❌ No     |

---

## 📄 JSON Mapping Format

```json
{
  "pipelines/sample_pipeline.py": "pipelines/sample_pipeline.yaml",
  "other/example.py": "other/example.yaml"
}
```

---

## 🚀 Usage

```yaml
jobs:
  validate-kfp:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout source code
        uses: actions/checkout@v4

      - name: Validate compiled pipelines
        uses: hbelmiro/validate-kfp-compiled-files@<commit-sha>
        with:
          pipelines-map-file: './.github/pipelines-map.json'
          requirements-file: './pipeline/requirements.txt'
          extra-compile-args: '--some-kfp-flag value'
```
