# LLMS File Builder

> Transform your Screaming Frog SEO crawls into AI-optimized LLMS.txt files for enhanced discoverability on ChatGPT, Claude, Perplexity, and other AI search engines.

[![Python](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--3.5-orange.svg)](https://openai.com/)

## Why LLMS.txt?

AI search engines are increasingly used for finding businesses and services. An LLMS.txt file helps these AI systems understand your website's structure and content, similar to how robots.txt helps traditional search engines. This tool automates the creation of these files from your existing SEO crawl data, with built-in AI optimization for maximum discoverability.

## Architecture

```mermaid
graph TB
    subgraph "Input"
        A[Screaming Frog CSV Export] --> B[CSV Processor]
    end

    subgraph "Processing Pipeline"
        B --> C{Validation}
        C -->|Valid| D[Filter & Clean]
        C -->|Invalid| E[Error + Advice]
        D --> F[Deduplication]
        F --> G[Pattern-based Categorizer]
        G --> H[AI Content Optimizer]
        H --> I[LLMS Generator]
    end

    subgraph "Output"
        I --> L[LLMS.txt]
        L --> N[AI Search Engines]
    end

    style A fill:#e1f5fe
    style L fill:#c8e6c9
    style N fill:#fff3e0
```

### Component Overview

| Component | Purpose | Key Features |
|-----------|---------|--------------|
| **CSV Processor** | Validates and cleans Screaming Frog exports | Filters non-content pages, quality scoring, duplicate removal |
| **Categorizer** | Groups pages into logical sections | Pattern-based matching, medical/healthcare focus, AI-enhanced descriptions |
| **LLMS Generator** | Creates final LLMS.txt output | Markdown formatting, AI-optimized content, validation |
| **Streamlit App** | Password-protected web interface | Drag-and-drop upload, real-time preview, quality analysis |

## Quick Start

### Prerequisites

- Python 3.8+
- Screaming Frog SEO Spider (for crawling websites)
- OpenAI API key (**required** for AI-enhanced descriptions)

### Installation

```bash
# Clone the repository
git clone https://github.com/reallyreallyryan/llms-file-builder.git
cd llms-file-builder

# Create virtual environment
python -m venv .venv

# Activate virtual environment
# On Windows:
.venv\Scripts\activate
# On macOS/Linux:
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### API Key Setup

The OpenAI API key is **required**. Set it up using one of these methods:

**Method 1 — Streamlit secrets (recommended for the web interface):**

```bash
mkdir -p .streamlit
echo 'OPENAI_API_KEY = "sk-..."' >> .streamlit/secrets.toml
echo 'app_password = "your-password"' >> .streamlit/secrets.toml
```

**Method 2 — Environment file (for CLI usage):**

```bash
cp .env.example .env
# Edit .env and add your OpenAI API key
```

## Usage Guide

### Step 1: Export from Screaming Frog

**Important**: Export only HTML pages for best results!

1. Open Screaming Frog SEO Spider
2. Crawl your website
3. Click the **"Internal"** tab
4. Use the **Filter dropdown -> Select "HTML"**
5. Go to **File -> Export** -> Save as CSV

### Step 2: Generate LLMS.txt

#### Option A: Command Line Interface

```bash
# Basic usage (includes AI optimization)
python run.py data/your-crawl.csv

# Preview without saving
python run.py data/your-crawl.csv --preview

# Custom output filename
python run.py data/your-crawl.csv --output mysite_llms

# Validate CSV quality without processing
python run.py data/your-crawl.csv --validate-only

# Force processing even if CSV quality is poor
python run.py data/your-crawl.csv --force
```

#### Option B: Web Interface

The web interface is password-protected. Set `app_password` in `.streamlit/secrets.toml` before launching.

```bash
# Launch the Streamlit app
streamlit run app.py

# Then open http://localhost:8501 in your browser
```

#### Option C: Dev Container (GitHub Codespaces / VS Code)

A dev container is included for quick setup. Open the project in GitHub Codespaces or VS Code with the Dev Containers extension — dependencies install automatically and the Streamlit app launches on port 8501.

### Step 3: Deploy LLMS.txt

Place the generated `LLMS.txt` file in your website's root directory (e.g., `https://yoursite.com/llms.txt`)

## Configuration

### Default Category Patterns

The tool uses intelligent pattern matching to categorize pages:

```python
CATEGORY_PATTERNS = {
    "Services": ["services", "therapy", "treatment", "procedure"],
    "Areas Treated": ["conditions", "pain", "symptoms"],
    "Blog": ["blog", "article", "news", "insights"],
    "Providers": ["doctor", "physician", "team", "staff"],
    "Locations": ["location", "office", "clinic", "directions"],
    "Patient Resources": ["forms", "insurance", "faq", "appointment"],
    "About": ["about", "mission", "values", "history"]
}
```

### Customizing Categories

Edit `backend/categorizer.py` to add your own patterns:

```python
# Add industry-specific patterns
CUSTOM_PATTERNS = {
    "Products": ["product", "shop", "store", "catalog"],
    "Case Studies": ["case-study", "success-story", "client"],
    "Support": ["help", "support", "documentation", "guide"]
}

categorizer.update_patterns(CUSTOM_PATTERNS)
```

## Output Format

### LLMS.txt Structure

```markdown
# Your Site Name

> Brief description of your website and its purpose

## Services
- [Service Name](https://site.com/service): AI-optimized description of the service
- [Another Service](https://site.com/service-2): What this service offers

## Locations
- [City Location](https://site.com/locations/city): Address and specialties offered

## Blog
- [Article Title](https://site.com/blog/article): Key insights and topics covered
```

## Built-in AI Enhancement

The tool automatically:

1. **Preserves** accurate pattern-based categorization
2. **Enhances** titles and descriptions for AI search visibility
3. **Optimizes** content specifically for LLMS.txt format
4. **Maximizes** discoverability in ChatGPT, Claude, Perplexity, and other AI search engines

### AI Optimization Focus

- **Services**: Emphasizes solutions and outcomes
- **Providers**: Highlights expertise and specializations
- **Locations**: Includes accessibility and service availability
- **Blog**: Focuses on educational value and key takeaways

## Quality Assurance

The tool includes comprehensive quality checks:

### CSV Quality Analysis

- **Detects** improperly filtered exports (images, CSS, JS files)
- **Scores** export quality (0-100)
- **Provides** specific export instructions
- **Suggests** improvements for better results

### Content Validation

- **Removes** duplicate URLs and titles
- **Filters** non-indexable pages
- **Improves** empty descriptions
- **Validates** output structure

## API Reference

### LLMSProcessor

```python
from backend import LLMSProcessor

# Initialize processor (AI optimization is always enabled)
processor = LLMSProcessor(
    output_dir="exports",
    api_key="your-api-key"  # Uses env var by default
)

# Process file
result = processor.process_file(
    csv_path="data/crawl.csv",
    preview_only=False,
    custom_filename="mysite"
)

# Result structure
{
    "success": True,
    "files": {
        "txt_path": "exports/mysite.txt"
    },
    "stats": {
        "total_rows": 500,
        "indexable_pages": 300,
        "unique_pages": 250
    },
    "categories": {
        "Services": 45,
        "Blog": 120,
        "Locations": 10
    }
}
```

### Categorizer

```python
from backend import Categorizer

# AI enhancement is always enabled
categorizer = Categorizer()
categorizer.update_patterns({
    "Custom Category": ["pattern1", "pattern2"]
})

# Categorize pages
categorized = categorizer.categorize_pages(
    pages=[...],
    site_metadata={...}
)
```

## Testing

There is no formal test suite yet. To verify your setup works correctly:

```bash
# Validate a CSV file without processing
python run.py data/your-crawl.csv --validate-only

# Preview output without saving
python run.py data/your-crawl.csv --preview
```

## Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| **Too many non-content files** | Re-export with HTML filter in Screaming Frog |
| **Missing columns error** | Ensure you're exporting from the "Internal" tab |
| **API key error** | Verify your OpenAI API key is set in `.env` or `.streamlit/secrets.toml` |
| **Empty descriptions** | Check if meta descriptions exist in your crawl |

### Debug Mode

```bash
# Enable verbose logging
export LOG_LEVEL=DEBUG
python run.py data/crawl.csv
```

## Roadmap

- [ ] Multi-language support
- [ ] Custom industry templates
- [ ] Bulk processing mode
- [ ] API endpoint
- [ ] WordPress plugin
- [ ] Chrome extension
- [ ] Advanced analytics dashboard

## License

This project is MIT licensed.

## Acknowledgments

- OpenAI for GPT-3.5 API
- Screaming Frog for the excellent SEO Spider tool
- The LLMS.txt specification creators

## Support

- **Issues**: [GitHub Issues](https://github.com/reallyreallyryan/llms-file-builder/issues)
- **Discussions**: [GitHub Discussions](https://github.com/reallyreallyryan/llms-file-builder/discussions)

---

Made with care for the AI-first web by Ryan K
