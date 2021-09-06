# Newman Runnar Action
This action provides a mechanism to run Postman test pack using the Newman runner. To accomplish this, it uses environment files that have had the secrets redacted within the repo, for hydration by org level secrets.

The following inputs are required:
- `aws-key`: AWS Access Key
- `aws-secret`: AWS Secret
- `vending-token`: Token to be used by credential vending service (or set to `na` if not required). More documentation on this to be added later.
- `tests-file`: Repo path to the tests file exported by Postman
- `env-file`: Repo path to the redacted environment file exported by Postman

Example job:

```yaml
name: Regression tests
on:
  workflow_run:
    workflows: ["Build, SM, Deploy"]
    branches: [dev]
    types: 
      - completed
  workflow_dispatch:

jobs:
  run-regression-tests:
    runs-on: ubuntu-latest
    name: Execute Newman scripts
    steps:
    - name: Run Newman
      uses: askporter/newman-action@v1
      with:
        aws-key: {{ secrets.AUTO_TEST_DEV_ACCESS_KEY }}
        aws-secret: {{ secrets.AUTO_TEST_DEV_SECRET }}
        vending-token: {{ secrets.AUTO_TEST_DEV_VENDING_TOKEN }}
        tests-file: postman/active_regression_tests.json
        env-file: postman/envs/dev_eu-west-1.json 
```