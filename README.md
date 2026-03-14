# lottery-form

Google Apps Script web app — a single-page form for BHS students to rank Learning Communities (LCs) for the lottery.

## Clasp Commands

clasp is installed locally. Use `./node_modules/.bin/clasp` (or `npx clasp`):

```bash
./node_modules/.bin/clasp pull      # pull latest from Apps Script
./node_modules/.bin/clasp push      # push local changes to Apps Script
./node_modules/.bin/clasp open      # open project in Apps Script editor
```

There is no build step or test suite — changes are pushed directly to Google Apps Script and tested live.

## Managing Deployments

### Update the test deployment

The test deployment always runs the latest pushed code. Just push:

```bash
./node_modules/.bin/clasp push
```

### Update an existing deployment to new code

This is equivalent to opening a deployment in "Manage Deployments" and clicking "New version".

```bash
# 1. Push your changes
./node_modules/.bin/clasp push

# 2. Create an immutable version snapshot
./node_modules/.bin/clasp create-version "description of changes"

# 3. Find the new version number and your deployment ID
./node_modules/.bin/clasp list-versions
./node_modules/.bin/clasp list-deployments

# 4. Point the existing deployment at the new version
./node_modules/.bin/clasp update-deployment <deploymentId> -V <versionNumber> -d "description"
```

### Create a brand new deployment

```bash
# 1. Push your changes and create a version (steps 1-2 above)

# 2. Create a new deployment pointing at that version
./node_modules/.bin/clasp create-deployment -V <versionNumber> -d "description"
```
