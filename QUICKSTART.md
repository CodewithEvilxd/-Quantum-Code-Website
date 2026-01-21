# Quantum Code Quick Start Guide

## 🚀 What is Quantum Code?

Quantum Code is an **enterprise-grade AI orchestration platform** that enhances code analysis through **multi-model intelligence**. Instead of relying on a single AI model, it orchestrates multiple AI providers (OpenAI GPT, Anthropic Claude, Google Gemini) to deliver more accurate, comprehensive code reviews and insights.

### Key Innovation
- **Parallel AI Analysis**: Runs multiple AI models simultaneously
- **Consensus Validation**: Cross-verifies results across different models
- **Security-First**: OWASP Top 10 vulnerability detection
- **Enterprise Ready**: Production-grade architecture

---

## 📦 Installation

### Option 1: From PyPI (Recommended)
```bash
pip install quantum-code
```

### Option 2: From Source
```bash
git clone https://github.com/Codewithevilxd/quantum-code.git
cd quantum-code
pip install -e .
```

### Verify Installation
```bash
# Check CLI
quantum --help

# Or using Python module
python -m quantum_code.cli --help
```

---

## 🔑 API Key Setup

Quantum Code requires API keys from at least one AI provider. Create a `.env` file in your project directory or home folder.

### Step 1: Create Environment File
```bash
# In your project directory
cp .env.example .env

# Or in your home directory for global use
# ~/.quantum_code/.env
```

### Step 2: Add API Keys

#### OpenAI (Recommended for beginners)
```bash
OPENAI_API_KEY=sk-proj-your-openai-key-here
```

#### Anthropic Claude (Best for code analysis)
```bash
ANTHROPIC_API_KEY=sk-ant-api03-your-anthropic-key-here
```

#### Google Gemini (Fast and cost-effective)
```bash
GEMINI_API_KEY=AIzaSy-your-gemini-key-here
```

#### Multiple Providers (Recommended)
```bash
# Primary providers
OPENAI_API_KEY=sk-proj-...
ANTHROPIC_API_KEY=sk-ant-api03-...
GEMINI_API_KEY=AIzaSy-...

# Optional: Enterprise providers
AZURE_OPENAI_API_KEY=...
AWS_ACCESS_KEY_ID=...
GOOGLE_CLOUD_PROJECT=your-project
```

### Step 3: Verify Keys
```bash
# Test configuration
python -c "from quantum_code.settings import settings; print('Keys loaded successfully')"
```

---

## 🎯 Usage Modes

### Mode 1: CLI Code Review (Standalone)

#### Basic Code Review
```bash
# Review a single file
quantum src/main.py

# Review entire directory
quantum src/

# Review specific file types
quantum src/ --include "*.py" "*.js"
```

#### Advanced Options
```bash
# Use specific AI model
quantum src/ --model claude-sonnet

# Use multiple models
quantum src/ --models gpt-4o,claude-sonnet,gemini-pro

# JSON output for CI/CD
quantum src/ --json > review-results.json

# Verbose logging
quantum src/ -v

# Specify project root
quantum src/ --base-path /path/to/project
```

### Mode 2: MCP Server (Claude Code Integration)

#### Step 1: Configure Claude Code
Add to your `~/.claude.json`:
```json
{
  "mcpServers": {
    "quantum": {
      "command": "python",
      "args": ["-m", "quantum_code.server"]
    }
  }
}
```

#### Step 2: Restart Claude Code
```bash
# Restart Claude Code to load MCP server
claude
```

#### Step 3: Use in Claude Chat
```
Can you quantum codereview this authentication module?
Can you quantum compare these two architecture approaches?
Can you quantum debate the best testing strategy?
```

---

## 🛠️ Available Tools

### 1. Code Review (`codereview`)
Comprehensive code analysis covering:
- **Quality**: Code structure, readability, maintainability
- **Security**: OWASP Top 10 vulnerabilities, injection risks
- **Performance**: Algorithm efficiency, resource usage
- **Architecture**: Design patterns, scalability concerns

**Usage:**
```bash
# CLI
quantum src/auth.py

# Claude Code
"quantum codereview this authentication module for security issues"
```

### 2. Chat (`chat`)
Interactive AI assistance with:
- **Context Awareness**: Repository and file understanding
- **Multi-turn Conversations**: Maintains conversation history
- **Code Examples**: Provides relevant code snippets

**Usage:**
```bash
# Claude Code
"quantum chat how does this authentication flow work?"
"quantum chat suggest improvements for this API design"
```

### 3. Compare (`compare`)
Side-by-side analysis of:
- **Architecture Approaches**: Different design patterns
- **Technology Choices**: Framework comparisons
- **Implementation Strategies**: Various solutions

**Usage:**
```bash
# Claude Code
"quantum compare REST API vs GraphQL for this use case"
"quantum compare different state management approaches"
```

### 4. Debate (`debate`)
Advanced consensus building:
- **Multi-agent Debate**: Models argue different perspectives
- **Step 1**: Independent analysis from each model
- **Step 2**: Cross-examination and critique
- **Final Consensus**: Weighted recommendation

**Usage:**
```bash
# Claude Code
"quantum debate the best database choice for this application"
"quantum debate microservices vs monolith architecture"
```

---

## ⚙️ Configuration

### Model Aliases
Use short names instead of full model identifiers:

| Alias | Full Model | Provider |
|-------|------------|----------|
| `mini` | gpt-4o-mini | OpenAI |
| `gpt` | gpt-4o | OpenAI |
| `sonnet` | claude-sonnet-4.5 | Anthropic |
| `haiku` | claude-haiku-4.5 | Anthropic |
| `gemini` | gemini-pro | Google |
| `flash` | gemini-flash | Google |

### Custom Configuration
Create `~/.quantum_code/config.yaml`:
```yaml
version: "1.0"
models:
  my-custom-model:
    litellm_model: openai/gpt-4o
    aliases:
      - custom
    notes: "My custom GPT-4o configuration"
```

---

## 🔍 Example Workflows

### Scenario 1: New Feature Code Review
```bash
# 1. Write your code
# 2. Run comprehensive review
quantum src/new_feature.py --models claude-sonnet,gpt-4o

# 3. Address issues found
# 4. Run final verification
quantum src/new_feature.py
```

### Scenario 2: Architecture Decision
```bash
# In Claude Code:
"quantum compare using Redis vs PostgreSQL for session storage"
"quantum debate the best caching strategy for this high-traffic API"
```

### Scenario 3: Security Audit
```bash
# Security-focused review
quantum src/auth.py src/api.py --focus security

# OWASP compliance check
quantum src/ --security-scan
```

### Scenario 4: CI/CD Integration
```yaml
# .github/workflows/code-review.yml
name: AI Code Review
on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Quantum Code Analysis
        run: |
          pip install quantum-code
          quantum src/ --json --models claude-sonnet,gpt-4o > review.json
```

---

## 🐛 Troubleshooting

### "No API key found"
```bash
# Check if .env file exists
ls -la .env

# Verify key format
cat .env | grep OPENAI_API_KEY

# Test key loading
python -c "from quantum_code.settings import settings; print(settings.openai_api_key[:10] + '...')"
```

### "Model not available"
```bash
# Check available models
quantum --list-models

# Update to supported model
quantum src/ --model gpt-4o-mini
```

### MCP Server not connecting
```bash
# Test MCP server directly
python -m quantum_code.server

# Check Claude Code config
cat ~/.claude.json

# Restart Claude Code completely
```

### Performance Issues
```bash
# Use fewer models for faster results
quantum src/ --models gpt-4o-mini,gemini-flash

# Use single model for quick checks
quantum src/ --model gemini-flash
```

---

## 📊 Understanding Results

### Code Review Output
```
🔍 Code Quality: 8.5/10
✅ Good separation of concerns
✅ Clear naming conventions
⚠️  Consider adding input validation

🛡️ Security Score: 9.2/10
✅ No SQL injection vulnerabilities
✅ Proper authentication checks
⚠️  Add rate limiting for API endpoints

⚡ Performance: 7.8/10
✅ Efficient algorithms used
⚠️  Consider caching for frequent queries
```

### Consensus Indicators
- **High Confidence**: All models agree
- **Medium Confidence**: Majority agreement with minor differences
- **Low Confidence**: Significant disagreement (requires human review)

---

## 🎯 Best Practices

### For Development Teams
1. **Use multiple models** for important reviews
2. **Set up CI/CD integration** for automated reviews
3. **Configure model aliases** for team consistency
4. **Review consensus results** carefully

### For Individual Developers
1. **Start with single model** for quick feedback
2. **Use compare mode** for architecture decisions
3. **Leverage chat mode** for implementation guidance
4. **Run security scans** before deployment

### Cost Optimization
1. **Use appropriate models** for task complexity
2. **Batch reviews** instead of reviewing single files
3. **Configure rate limits** to control API usage
4. **Use cached results** when possible

---

## 📈 Advanced Features

### Custom Model Integration
Add proprietary or specialized models:
```yaml
# ~/.quantum_code/config.yaml
version: "1.0"
models:
  my-local-llm:
    provider: cli
    cli_command: ollama
    cli_args: ["run", "codellama"]
    aliases: ["local"]
```

### Enterprise Deployment
For organizations with custom requirements:
- **Private model endpoints**
- **Custom security policies**
- **Audit logging integration**
- **SLA-based support**

---

## 🤝 Support & Community

### Getting Help
- **Documentation**: https://github.com/Codewithevilxd/quantum-code
- **Issues**: https://github.com/Codewithevilxd/quantum-code/issues
- **Discussions**: GitHub Discussions tab

### Enterprise Support
For commercial support and custom integrations:
- **Email**: enterprise@codewithevilxd.com
- **Features**: Custom model integration, enterprise deployment, training

---

**Quantum Code** - *Multi-Model AI Orchestration for Superior Code Analysis*

**Ready to supercharge your code reviews?** 🚀

```bash
pip install quantum-code
quantum --help