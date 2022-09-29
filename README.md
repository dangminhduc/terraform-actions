# terraform-actions
GitHub Composite Actions for Terraform workflows.
Using exclusively with AWS.

# How to use

Example: 
```yaml
jobs:
  plan:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
      pull-requests: write
    steps:
      - name: Checkout
        uses: actions/checkout@v3.0.2

      - name: Checkout composite action
        uses: actions/checkout@v3.0.2
        with:
          repository: dangminhduc/terraform-actions
          ref: v1.0.0
          path: ./.github

      - uses: ./.github/actions/{plan or apply}
        with:
          stage: 
          working-directory: 
          terraform-version: ${{ env.TERRAFORM_VERSION }}
          execution-role: ${{ env.EXECUTION_ROLE }}
          project-name: ${{ env.PROJECT_NAME }}
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

* Input:

|Input|Explanation|Ex|
|---|---|---|
|stage|Environment name(if you have multile environment)|stg or prod|
|working-directory|Directory that contains terraform code|`envs/stg`or `envs/prod`|
|terraform-version|Which terraform version to use|1.1.1|
|execution-role|IAM Role which terraform will use to make changes to your infrastructure|arn:aws:iam::{account-id}:role/terraform-execution-role|
|github-token|For posting plan result in Pull Requests, the GITHUB_TOKEN is required|leave it as ${{ secrets.GITHUB_TOKEN }}|
