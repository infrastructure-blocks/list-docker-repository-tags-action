# list-docker-repository-tags-action

A GithubAction to list a Docker repository tags. It leverages the Docker registry API V2 to do so.
It requires the use of an authorization token (passed as a `Bearer` token) to the specified registry and
extracts the tags of the requested repository.

## Usage

```yaml
jobs:
  example-with-ecr-public:
    steps:
      - name: Get token from ECR public
        id: get-token
        run: |
          response=$(curl -L -X GET https://public.ecr.aws/token/)
          token=$(echo ${response} | jq -e -r '.token')
          echo "token=${token}" >> "${GITHUB_OUTPUT}"
      - uses: infrastructure-blocks/list-docker-repository-tags@v1
        with:
          token: ${{ steps.get-token.outputs.token }}
          registry: public.ecr.aws
          repository: your/repository
```

## Development

### Releases

Releases of this action is automated by the CI. It is required to provide a semantic version bump as a label on the
PR. Upon merges, the CI will generate/update the appropriate tags. See [here](https://github.com/infrastructure-blocks/git-tag-semver-from-label-action) for more information.
