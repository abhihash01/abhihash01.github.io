# Personal Website Builder Files

This repository contains the builder files for my personal website. Below are the steps, guidelines, and common debug points for managing and deploying the website.

## Workflow for Making Changes

### 1. Test Locally
After making changes, test them locally using the following command:

```bash
hugo server
```


This will serve the website locally (usually at `http://localhost:1313`). Verify that the changes are as expected.

### 2. Build the Website
If the changes are satisfactory, clear the `public` subfolder and build the site using:

```bash
hugo
```


This command generates the site using the base URL specified in the `config` directory. Note that local builds use `localhost` as the base URL, while production builds use the configured base URL.

### 3. Push Changes
Push your changes to the repository. The GitHub Actions workflow will automatically handle deployment.

---

## Common Debug Points

### For Local Testing
- Check the contents of the `public` directory to ensure files are being generated correctly.

### For Production Builds
- Verify that the `public` directory contains all components built with the correct base URL.

### For Deployment via GitHub Actions
- Inspect the created artifact in the workflow logs. It should contain all components of the `public` directory after a successful build.

---

## Troubleshooting

### 1. Missing Codes (e.g., StreetMap or Other Submodule Content)
This issue occurs when Git submodule contents are empty, leaving only their links behind. To resolve this, update submodules recursively:


```bash
git submodule update --recursive
```


### 2. Page Attributes Not Updating
If updates to page attributes aren't reflected in your browser, perform a hard reload using:

- **Windows/Linux:** `Ctrl + Shift + R`
- **Mac:** `Cmd + Shift + R`

---

## Alternative Deployment Method

If you wish to deploy without using GitHub Actions:

1. Build the site with the correct base URL:

```markdown
hugo
```

2. Copy all contents of the `public` directory into the `gh-pages` branch.
3. Deploy directly from this branch by enabling GitHub Pages for it in your repository settings.

---

## Additional Notes

- A Git submodule has been added in the themes repository. Ensure submodules are always updated when pulling or cloning this repository.

- Change log: to replace the theme icons directory, copy pasted the icons folder from the static directory of the theme to the static directory of website root and replaced the images. 
