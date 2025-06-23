> Reference: [GitLab CI / CD Predefined Variables](https://docs.gitlab.com/ci/variables/predefined_variables/)

#### Useful variables
- `CI_COMMIT_REF_NAME` - name of the branch that's being built
- `CI_DEFAULT_BRANCH` - the default protected branch (`main`)
- `CI_ENVIRONMENT_URL` - the deployment URL of the environment

#### Environment-specific Variables
You can define variables that are only visible to a specific environment. This is useful in cases where you have a different value per environment (tokens, secrets, etc.). To do this, go to the project's `Settings > CI/CD`, then when creating a CI/CD variable, you can set it specific to an environment in the dropdown.

![[Pasted image 20250618221210.png]]

