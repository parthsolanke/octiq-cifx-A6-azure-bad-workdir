# CI detection fixture

Npm@1 task with `workingDirectory: $(Build.SourcesDirectory)/app` - an Azure runtime variable the detector cannot resolve. Must be 'unknown', never attributed to the repo root.

Expected verdict line in the Octiq chat:

    Tests not run - CI runs `npm run test`, but its working directory or arguments cannot be resolved
