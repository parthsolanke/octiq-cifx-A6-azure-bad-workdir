# octiq-cifx-A6-azure-bad-workdir

A minimal Node.js project used as an Azure Pipelines fixture for Octiq's CI
detection. It exists to exercise the case where a pipeline invokes `npm run test`
from a working directory that cannot be resolved at detection time.

## Project

A private npm package (`fixture`, version `1.0.0`) with two scripts:

- `npm run build` — prints `building` (placeholder, no real build output).
- `npm run test` — runs `node test.js`.

`test.js` is a trivial but real test: it exits non-zero if `1 + 1 !== 2` and
otherwise prints `1 passing`.

## Azure Pipeline

`azure-pipelines.yml` triggers on `main` and runs a single `Npm@1` task on
`ubuntu-latest`:

```yaml
- task: Npm@1
  inputs:
    command: custom
    customCommand: run test
    workingDirectory: $(Build.SourcesDirectory)/app
```

The `workingDirectory` uses the Azure runtime variable
`$(Build.SourcesDirectory)/app`, which a static detector cannot resolve to a
concrete path. The fixture's purpose is to ensure the detector reports the
working directory (and therefore the test invocation) as **unknown** rather
than attributing it to the repository root, where `package.json` lives.

## Expected verdict

    Tests not run - CI runs `npm run test`, but its working directory or arguments cannot be resolved
