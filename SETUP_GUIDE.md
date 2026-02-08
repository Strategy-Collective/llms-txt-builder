# LLMS File Builder - Quick Reference

## Installation (One Time)

```bash
git clone https://github.com/reallyreallyryan/llms-file-builder.git
cd llms-file-builder
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### API Key Setup (Required)

Set your OpenAI API key using one of these methods:

```bash
# Option 1: .env file (for CLI)
echo "OPENAI_API_KEY=sk-..." > .env

# Option 2: Streamlit secrets (for web interface)
mkdir -p .streamlit
echo 'OPENAI_API_KEY = "sk-..."' >> .streamlit/secrets.toml
echo 'app_password = "your-password"' >> .streamlit/secrets.toml
```

## Screaming Frog Export (Critical!)

1. Open Screaming Frog -> Crawl your site
2. Click **"Internal"** tab
3. **Filter -> HTML** (This is crucial!)
4. **File -> Export** -> Save as CSV

## Generate LLMS.txt

```bash
# Standard usage (AI optimization is always enabled)
python run.py data/your-site.csv
```

### Web Interface

```bash
streamlit run app.py
# Open http://localhost:8501
# Requires app_password in .streamlit/secrets.toml
```

## Common Commands

```bash
# Preview without saving
python run.py data/site.csv --preview

# Custom filename
python run.py data/site.csv --output mysite

# Validate CSV only
python run.py data/site.csv --validate-only

# Force processing (skip quality warnings)
python run.py data/site.csv --force
```

## Output Files

```
exports/
└── LLMS.txt      # Upload to yoursite.com/llms.txt
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| "Missing columns" | Re-export from "Internal" -> "HTML" |
| "Too many images" | You didn't filter by HTML |
| "API key" error | Verify key in `.env` or `.streamlit/secrets.toml` |

## Category Patterns

Default categories:

- **Services**: /services/, therapy, treatment
- **Areas Treated**: /conditions/, pain, symptoms
- **Blog**: /blog/, article, news
- **Providers**: /physicians/, doctor, team
- **Locations**: /locations/, office, clinic
- **Patient Resources**: forms, insurance, faq
- **About**: /about/, mission, values

## Quality Indicators

- **Good CSV**: Quality score > 80, mostly HTML pages
- **OK CSV**: Quality score 60-80, some non-content
- **Poor CSV**: Quality score < 60, many images/assets

## Need Help?

- Use `--validate-only` to check CSV quality
- Use `--preview` to see output before saving
- Use `--force` to process despite quality warnings
- Check `exports/` folder for output files

**Pro Tip**: Always use Screaming Frog's HTML filter for best results!
