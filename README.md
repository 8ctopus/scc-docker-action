# SCC Docker Action

This action counts the lines of code and performs complexity analysis using scc. (https://github.com/boyter/scc)

## Inputs

### `args`

**Required** The CLI args corresponding to the SCC tool

## Outputs

### `scc.json`

The result of the scc analysis in JSON format as a file in the root folder of the runner. This file can be uploaded as an artifact in the same workflow or can be used as is in the same job context.

## Example usage

_.github/workflows/main.yml_

```
on: [push]

jobs:
  scc_job:
    runs-on: ubuntu-latest
    name: SCC analysis
    steps:
      - name: Checkout
        uses: actions/checkout@v2
        
      - name: Execute scc.
        id: scc
        uses: hdmsantander/scc-docker-action@3.0
        with:
          args: ${{ env.workspace }} src --ci -i java -f json -o scc.json

      - name: Parse scc output
        id: parse_scc
        run: echo "::set-output name=loc::$(jq '.[0].Code' scc.json)"

      - name: Make a lines of code badge
        uses: emibcn/badge-action@v2.0.2
        with:
          label: Lines of Code
          status: ${{ steps.parse_scc.outputs.loc }}
          color: 'blue'
          path: .github/lines.svg
```
