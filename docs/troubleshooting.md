# Troubleshooting Guide for Bruno AI Assistants

This guide helps you resolve common issues when using AI assistants with Bruno API Client.

## 🔍 Quick Diagnostics

### Is Your AI Assistant Finding the Prompt Files?

Run this checklist based on your AI assistant:

| AI Assistant | Expected File Location | How to Verify |
|--------------|------------------------|---------------|
| Cursor | `.cursorrules` | File exists in project root |
| GitHub Copilot | `.github/copilot-instructions.md` | File exists in `.github/` directory |
| VS Code Extensions | `.vscode/ai-instructions.md` | File exists in `.vscode/` directory |
| Continue | `.continue/config.json` | File exists in `.continue/` directory |
| Codeium | `.codeium/context.md` | File exists in `.codeium/` directory |

### Quick Check Command
```bash
# Run this in your Bruno project root
ls -la .cursorrules .github/copilot-instructions.md .vscode/ai-instructions.md .codeium/context.md .continue/config.json 2>/dev/null || echo "Some files are missing"
```

## 🚨 Common Issues

### 1. AI Not Understanding Bruno Syntax

**Symptoms:**
- AI suggests JSON format instead of .bru format
- Incorrect variable syntax (not using `{{variableName}}`)
- Missing or incorrect meta blocks
- Wrong authentication patterns

**Solutions:**

#### For All AI Assistants
1. **Verify prompt file exists** in the correct location
2. **Restart your editor** to reload the configuration
3. **Be explicit in prompts**: Mention "Bruno .bru format" explicitly
4. **Provide examples**: Reference existing .bru files in your project

#### Cursor-Specific
```bash
# Verify .cursorrules exists
ls -la .cursorrules

# Re-download if missing
curl -fsSL https://raw.githubusercontent.com/bruno-collections/ai-assistant-prompts/main/prompts/cursor/.cursorrules -o .cursorrules
```

#### GitHub Copilot-Specific
```bash
# Verify copilot-instructions.md exists
ls -la .github/copilot-instructions.md

# Re-download if missing
mkdir -p .github
curl -fsSL https://raw.githubusercontent.com/bruno-collections/ai-assistant-prompts/main/prompts/copilot/.github/copilot-instructions.md -o .github/copilot-instructions.md
```

#### VS Code Extensions-Specific
```bash
# Verify ai-instructions.md exists
ls -la .vscode/ai-instructions.md

# Re-download if missing
mkdir -p .vscode
curl -fsSL https://raw.githubusercontent.com/bruno-collections/ai-assistant-prompts/main/prompts/vscode/.vscode/ai-instructions.md -o .vscode/ai-instructions.md
```

### 2. AI Suggestions Are Generic

**Symptoms:**
- Suggestions don't use Bruno-specific patterns
- Missing dynamic variables like `{{$randomEmail}}`
- No awareness of Bruno's JavaScript API (req, res, bru)
- Generic test assertions instead of Chai.js

**Solutions:**

1. **Update prompt files** to the latest version:
```bash
cd your-bruno-project
curl -fsSL https://raw.githubusercontent.com/bruno-collections/ai-assistant-prompts/main/install.sh | bash
```

2. **Use more specific prompts**:
   - ✅ "Create a Bruno .bru file with bearer authentication and Chai.js tests"
   - ❌ "Create an API request"

3. **Reference existing files**:
   - "Similar to login.bru, create a logout.bru file"
   - "Use the same pattern as get-user.bru"

4. **Provide context in comments**:
```bru
# This request uses OAuth2 bearer token authentication
# and includes comprehensive error handling
meta {
  name: Get User Profile
  type: http
}
```

### 3. Environment Variables Not Suggested

**Symptoms:**
- AI doesn't suggest `{{variableName}}` syntax
- Missing vars and vars:secret blocks
- Hardcoded values instead of variables

**Solutions:**

1. **Create example environment files** in your project:
```bru
# environments/development.bru
vars {
  baseUrl: https://api.dev.example.com
  apiVersion: v1
}

vars:secret [
  authToken,
  apiKey
]
```

2. **Mention environment variables** in your prompts:
   - "Use {{baseUrl}} for the API endpoint"
   - "Store the API key in vars:secret"

3. **Reference environment files**:
   - "Use variables from the development environment"

### 4. Tests Not Being Generated

**Symptoms:**
- AI doesn't add tests blocks
- Missing Chai.js assertions
- Generic or incorrect test syntax

**Solutions:**

1. **Explicitly request tests**:
   - "Add comprehensive tests using Chai.js assertions"
   - "Include tests for status code, response structure, and data validation"

2. **Provide test examples** in your project:
```bru
tests {
  test("Status is successful", function() {
    expect(res.status).to.be.oneOf([200, 201]);
  });
  
  test("Response has required fields", function() {
    expect(res.body).to.have.property("id");
    expect(res.body).to.have.property("name");
  });
}
```

3. **Reference existing tests**:
   - "Add tests similar to those in create-user.bru"

### 5. Scripts Not Using Bruno API Correctly

**Symptoms:**
- Incorrect usage of req, res, bru objects
- Wrong method names (e.g., `bru.setVariable` instead of `bru.setVar`)
- Missing error handling

**Solutions:**

1. **Verify prompt files are up to date** (they include the JavaScript API reference)

2. **Provide correct examples**:
```bru
script:pre-request {
  // Correct Bruno API usage
  bru.setVar("timestamp", Date.now());
  req.setHeader("X-Request-ID", bru.getVar("requestId"));
}

script:post-response {
  if (res.status === 200) {
    bru.setVar("userId", res.body.id);
    bru.setNextRequest("Get User Profile");
  }
}
```

3. **Reference the Bruno documentation**:
   - "Use bru.setVar() to set runtime variables"
   - "Use req.setHeader() to add request headers"

## 🔧 Editor-Specific Issues

### Cursor

**Issue**: Cursor not loading .cursorrules
- **Solution**: Ensure file is named exactly `.cursorrules` (no extension)
- **Solution**: Restart Cursor completely
- **Solution**: Check Cursor settings for custom rules path

### GitHub Copilot

**Issue**: Copilot not using instructions
- **Solution**: Verify GitHub Copilot is authenticated
- **Solution**: Check Copilot status in status bar
- **Solution**: Ensure file is at `.github/copilot-instructions.md`

### VS Code Extensions (Cline, Roo Code, etc.)

**Issue**: Extension not finding instructions
- **Solution**: Check extension-specific settings for instruction file paths
- **Solution**: Some extensions may use different file names
- **Solution**: Consult extension documentation

### Continue

**Issue**: Custom commands not showing
- **Solution**: Verify `.continue/config.json` is valid JSON
- **Solution**: Check Continue extension is activated
- **Solution**: Restart VS Code/JetBrains IDE

**Issue**: API key errors
- **Solution**: Verify API key format in config.json
- **Solution**: Check API key has correct permissions
- **Solution**: Test with a simple prompt

### Codeium

**Issue**: Context not being used
- **Solution**: Ensure `.codeium/context.md` exists
- **Solution**: Clear Codeium cache and restart
- **Solution**: Check Codeium output panel for errors

## 📝 Getting Better Results

### Prompt Engineering Tips

1. **Be Specific About Bruno**:
   - ✅ "Create a Bruno .bru file for user authentication"
   - ❌ "Create an API request"

2. **Mention Key Features**:
   - "Use bearer token authentication"
   - "Include Chai.js test assertions"
   - "Add pre-request script to set timestamp"

3. **Reference Existing Files**:
   - "Similar to create-user.bru, create update-user.bru"
   - "Use the same auth pattern as login.bru"

4. **Specify Environment**:
   - "Use variables from the development environment"
   - "Store secrets in vars:secret block"

### Project Structure Tips

1. **Use Descriptive File Names**:
   - ✅ `create-user.bru`, `get-user-profile.bru`
   - ❌ `request1.bru`, `test.bru`

2. **Add Comments**:
```bru
# OAuth2 authentication flow
# Step 1: Get access token
meta {
  name: Get Access Token
  type: http
}
```

3. **Keep Examples**:
   - Maintain a few well-documented example requests
   - AI assistants learn from existing patterns

## 🆘 Still Having Issues?

### Check for Updates
```bash
# Update to the latest prompt files
cd your-bruno-project
curl -fsSL https://raw.githubusercontent.com/bruno-collections/ai-assistant-prompts/main/install.sh | bash
```

### Verify File Contents
```bash
# Check if prompt files have content
wc -l .cursorrules .github/copilot-instructions.md .vscode/ai-instructions.md 2>/dev/null
```

### Community Support
- [Bruno Discord](https://discord.com/invite/KgcZUncAjX)
- [Bruno GitHub Discussions](https://github.com/usebruno/bruno/discussions)
- [Bruno Collections](https://github.com/bruno-collections)

### Report Issues
If you find bugs in the prompt files:
1. Open an issue at [bruno-ai-assistant-prompts](https://github.com/bruno-collections/ai-assistant-prompts/issues)
2. Include your AI assistant name and version
3. Provide example prompts and unexpected outputs
4. Share your prompt file location and contents (if possible)

## 📚 Additional Resources

- [Cursor Documentation](./cursor.md)
- [GitHub Copilot Documentation](./copilot.md)
- [VS Code Extensions Documentation](./vscode.md)
- [Continue Documentation](./continue.md)
- [Codeium Documentation](./codeium.md)
- [Bruno Official Docs](https://docs.usebruno.com)

