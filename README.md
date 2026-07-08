# SARIF Scanner & Uploader

A GitHub Actions workflow project for uploading and processing SARIF (Static Analysis Results Interchange Format) files to GitHub's code scanning.

## Features

✅ **Manual Trigger** - Trigger the workflow manually via GitHub Actions  
✅ **File Validation** - Checks if SARIF file exists and is valid JSON  
✅ **Artifact Storage** - Stores SARIF file as GitHub artifact (30-day retention)  
✅ **Code Scanning** - Uploads SARIF to GitHub's code scanning dashboard  
✅ **Summary Report** - Generates workflow summary with upload details  

## How to Use

### 1. Create GitHub Repository

```bash
cd sarif-scanner-project
git init
git add .
git commit -m "Initial commit: SARIF upload workflow"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/sarif-scanner-project.git
git push -u origin main
```

### 2. Manual Workflow Trigger

1. Go to your GitHub repository
2. Click **Actions** tab
3. Select **Upload SARIF Results** workflow
4. Click **Run workflow**
5. Enter the SARIF file path (e.g., `cx_result.sarif`)
6. Optionally enter a custom artifact name
7. Click **Run workflow**

### 3. Upload SARIF File

Before triggering the workflow, make sure your SARIF file is in the repository:

```bash
# Copy your SARIF file to the project root
cp /path/to/cx_result.sarif .

# Or commit it to the repo
git add cx_result.sarif
git commit -m "Add SARIF results"
git push
```

## File Structure

```
sarif-scanner-project/
├── .github/
│   └── workflows/
│       └── sarif-upload.yml       # Main workflow file
├── README.md                       # This file
├── .gitignore                      # Git ignore rules
└── cx_result.sarif                 # Your SARIF file (after generation)
```

## Workflow Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `sarif_file_path` | Path to SARIF file | Yes | `cx_result.sarif` |
| `artifact_name` | Name for the GitHub artifact | No | `SARIF Results` |

## Example SARIF File Paths

- `cx_result.sarif` - File in root directory
- `results/scan.sarif` - File in subdirectory
- `./output/checkmarx-results.sarif` - Relative path

## What Happens in the Workflow

1. **Checkout** - Gets the latest code from your repository
2. **File Check** - Verifies the SARIF file exists and shows its size
3. **Format Validation** - Ensures the file is valid JSON
4. **Artifact Upload** - Stores the file as a GitHub artifact (30 days)
5. **Code Scanning Upload** - Uploads to GitHub's Security → Code scanning tab
6. **Summary** - Generates a workflow summary report

## Viewing Results

### Artifacts
- Go to **Actions** → select a completed run → scroll down to **Artifacts**

### Code Scanning
- Go to **Security** → **Code scanning** tab to see uploaded vulnerabilities

## Troubleshooting

### SARIF file not found
- Ensure the file path is correct
- Check that the file is committed to the repository
- Use relative paths from the repository root

### Invalid JSON format
- Validate your SARIF file: `python3 -m json.tool cx_result.sarif`
- Ensure the file is valid SARIF format

### Upload fails
- Check that your repository has `Security` tab enabled
- Ensure the SARIF file is valid and properly formatted
- Review the workflow logs for detailed error messages

## Example: Checkmarx Integration

If you're using Checkmarx to generate SARIF files:

```bash
# Generate SARIF from Checkmarx
checkmarx-cli scan --output-format sarif --output-file cx_result.sarif

# Commit and push
git add cx_result.sarif
git commit -m "Add Checkmarx SARIF results"
git push

# Trigger workflow manually from GitHub Actions
```

## License

MIT

## Support

For issues or questions, open a GitHub issue in this repository.
