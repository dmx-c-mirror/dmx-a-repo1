# GitHub Actions Workflows

## Mirror Workflow

The `mirror.yml` workflow automatically mirrors every commit to a remote repository.

### Setup Instructions

To use this workflow, you need to configure the following secrets in your GitHub repository:

1. **MIRROR_REPO_URL** (Required)
   - The SSH URL of the remote repository you want to mirror to
   - Example: `git@github.com:username/mirror-repo.git`
   - Example: `git@gitlab.com:username/mirror-repo.git`

2. **MIRROR_SSH_KEY** (Required)
   - The private SSH key with write access to the mirror repository
   - Generate a new SSH key pair:
     ```bash
     ssh-keygen -t ed25519 -C "github-actions-mirror" -f mirror_key -N ""
     ```
   - Add the public key (`mirror_key.pub`) as a deploy key to the mirror repository with write access
   - Add the private key (`mirror_key`) content as the `MIRROR_SSH_KEY` secret

### How to Add Secrets

1. Go to your GitHub repository
2. Click on **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Add `MIRROR_REPO_URL` with the SSH URL of your mirror repository
5. Add `MIRROR_SSH_KEY` with your private SSH key content

### Workflow Behavior

- **Triggers**: On every push to any branch
- **Actions**: 
  - Checks out the full repository history
  - Configures SSH authentication
  - Pushes the current branch to the mirror repository
  - Optionally pushes all tags to the mirror repository
- **Force Push**: The workflow uses force push to ensure the mirror stays in sync

### Customization

You can customize the workflow by editing `.github/workflows/mirror.yml`:

- **Branch Filters**: Modify the `on.push.branches` section to mirror only specific branches
- **Multiple Mirrors**: Add additional remote repositories and push steps
- **Tag Mirroring**: Enable or disable tag mirroring by modifying the last step
- **Host Keys**: Add SSH host key scanning for other Git hosting providers (GitLab, Bitbucket, etc.)

### Troubleshooting

- **Permission Denied**: Ensure the SSH key has write access to the mirror repository
- **Unknown Host**: Add the appropriate host key scanning in the "Setup SSH key" step
- **Secret Not Found**: Verify that secrets are correctly configured in repository settings
