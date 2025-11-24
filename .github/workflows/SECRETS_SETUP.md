# GitHub Secrets Configuration

To enable automated deployment, add these secrets to your GitHub repository:

**Settings → Secrets and variables → Actions → New repository secret**

## Required Secrets

### FTP_SERVER
```
145.223.17.184
```

### FTP_USERNAME
```
u180508179.varnikaitconsulting.com
```

### FTP_PASSWORD
```
Knz;jALhyJZRO|9#
```

## How to Add Secrets

1. Go to: https://github.com/dixitsheta/VarnikaITConsulting/settings/secrets/actions
2. Click "New repository secret"
3. Name: `FTP_SERVER`, Value: `145.223.17.184`
4. Click "Add secret"
5. Repeat for `FTP_USERNAME` and `FTP_PASSWORD`

## Deployment Workflow

Once secrets are configured:

1. **Push to main branch** → Triggers automatic deployment
2. **Manual trigger** → Go to Actions tab → Run workflow
3. **Monitor progress** → Actions tab shows build/deploy status

## Deployment Time

- Build: ~30 seconds
- FTP Upload: ~1-2 minutes (depending on files changed)
- **Total: ~2-3 minutes** from commit to live site

## Security Note

⚠️ **Delete this file after adding secrets to GitHub**

```bash
git rm .github/workflows/SECRETS_SETUP.md
git commit -m "Remove secrets documentation"
git push
```
