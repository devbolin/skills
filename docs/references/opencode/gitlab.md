---
source: https://opencode.ai/docs/gitlab/
retrieved: 2026-05-12
type: archived
---

# GitLab

Use OpenCode in GitLab issues and merge requests.

OpenCode integrates with your GitLab workflow through your GitLab CI/CD pipeline or with GitLab Duo.

In both cases, OpenCode will run on your GitLab runners.

---

## GitLab CI

OpenCode works in a regular GitLab pipeline. You can build it into a pipeline as a CI component.

Here we are using a community-created CI/CD component for OpenCode — nagyv/gitlab-opencode.

---

### Features

- **Use custom configuration per job**: Configure OpenCode with a custom configuration directory, for example `./config/#custom-directory` to enable or disable functionality per OpenCode invocation.
- **Minimal setup**: The CI component sets up OpenCode in the background, you only need to create the OpenCode configuration and the initial prompt.
- **Flexible**: The CI component supports several inputs for customizing its behavior

---

### Setup

1. Store your OpenCode authentication JSON as a File type CI environment variables under **Settings** > **CI/CD** > **Variables**. Make sure to mark them as "Masked and hidden".

2. Add the following to your `.gitlab-ci.yml` file:

   ```yaml
   include:
     - component: $CI_SERVER_FQDN/nagyv/gitlab-opencode/opencode@2
       inputs:
         config_dir: ${CI_PROJECT_DIR}/opencode-config
         auth_json: $OPENCODE_AUTH_JSON
         command: optional-custom-command
         message: "Your prompt here"
   ```

---

## GitLab Duo

OpenCode integrates with your GitLab workflow. Mention `@opencode` in a comment, and OpenCode will execute tasks within your GitLab CI pipeline.

---

### Features

- **Triage issues**: Ask OpenCode to look into an issue and explain it to you.
- **Fix and implement**: Ask OpenCode to fix an issue or implement a feature. It will create a new branch and raise a merge request with the changes.
- **Secure**: OpenCode runs on your GitLab runners.

---

### Setup

OpenCode runs in your GitLab CI/CD pipeline. Here's what you'll need to set it up:

1. Configure your GitLab environment
2. Set up CI/CD
3. Get an AI model provider API key
4. Create a service account
5. Configure CI/CD variables
6. Create a flow config file

Example flow configuration:

```yaml
image: node:22-slim
commands:
  - echo "Installing opencode"
  - npm install --global opencode-ai
  - echo "Installing glab"
  - export GITLAB_TOKEN=$GITLAB_TOKEN_OPENCODE
  - apt-get update --quiet && apt-get install --yes curl wget gpg git && rm --recursive --force /var/lib/apt/lists/*
  - curl --silent --show-error --location "https://raw.githubusercontent.com/upciti/wakemeops/main/assets/install_repository" | bash
  - apt-get install --yes glab
  - echo "Configuring glab"
  - echo $GITLAB_HOST
  - echo "Creating OpenCode auth configuration"
  - mkdir --parents ~/.local/share/opencode
  - |
    cat > ~/.local/share/opencode/auth.json << EOF
    {
      "anthropic": {
        "type": "api",
        "key": "$ANTHROPIC_API_KEY"
      }
    }
    EOF
  - echo "Configuring git"
  - git config --global user.email "opencode@gitlab.com"
  - git config --global user.name "OpenCode"
  - echo "Testing glab"
  - glab issue list
  - echo "Running OpenCode"
  - |
    opencode run "You are an AI assistant helping with GitLab operations. ..."
  - git checkout --branch $CI_WORKLOAD_REF origin/$CI_WORKLOAD_REF
  - echo "Checking for git changes and pushing if any exist"
  - |
    if ! git diff --quiet || ! git diff --cached --quiet || [ --not --zero "$(git ls-files --others --exclude-standard)" ]; then
      echo "Git changes detected, adding and pushing..."
      git add .
      if git diff --cached --quiet; then
        echo "No staged changes to commit"
      else
        echo "Committing changes to branch: $CI_WORKLOAD_REF"
        git commit --message "Codex changes"
        echo "Pushing changes up to $CI_WORKLOAD_REF"
        git push https://gitlab-ci-token:$GITLAB_TOKEN@$GITLAB_HOST/gl-demo-ultimate-dev-ai-epic-17570/test-java-project.git $CI_WORKLOAD_REF
        echo "Changes successfully pushed"
      fi
    else
      echo "No git changes detected, skipping push"
    fi
variables:
  - ANTHROPIC_API_KEY
  - GITLAB_TOKEN_OPENCODE
  - GITLAB_HOST
```

---

### Examples

- **Explain an issue**: Add `@opencode explain this issue` in a GitLab issue.
- **Fix an issue**: Say `@opencode fix this` — OpenCode will create a new branch, implement changes, and open a merge request.
- **Review merge requests**: Leave `@opencode review this merge request` on a GitLab MR.
