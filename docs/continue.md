# Continue Extension Setup for Bruno

[Continue](https://continue.dev) is an open-source AI code assistant that works in VS Code and JetBrains IDEs. It can be configured with custom commands and context for Bruno development.

## 🚀 Quick Setup

### Automatic Installation
```bash
cd your-bruno-project
curl -fsSL https://raw.githubusercontent.com/bruno-collections/ai-assistant-prompts/main/install.sh | bash
```

### Manual Installation
1. Install the Continue extension:
   - **VS Code**: [Continue Extension](https://marketplace.visualstudio.com/items?itemName=Continue.continue)
   - **JetBrains**: [Continue Plugin](https://plugins.jetbrains.com/plugin/22707-continue)

2. Create the `.continue` directory:
```bash
mkdir -p .continue
```

3. Download the Bruno configuration:
```bash
curl -fsSL https://raw.githubusercontent.com/bruno-collections/ai-assistant-prompts/main/prompts/continue/.continue/config.json -o .continue/config.json
```

4. Update the config with your API keys (see Configuration section)

5. Restart your editor

## ✨ Features

### Custom Bruno Commands
The configuration includes specialized slash commands:

- `/bruno-request` - Generate a new Bruno API request file
- `/bruno-environment` - Create environment files with variables
- `/bruno-test` - Add comprehensive tests to requests
- `/bruno-script` - Add pre-request or post-response scripts
- `/bruno-chain` - Create request chaining workflows

### Bruno-Aware Context
- Automatic understanding of .bru file format
- JavaScript API knowledge (req, res, bru objects)
- Dynamic variables and environment management
- Testing patterns with Chai.js
- Request chaining and cookie management

## 🔧 Configuration

### Setting Up Your API Key

Edit `.continue/config.json` and add your API key:

```json
{
  "models": [
    {
      "title": "GPT-4",
      "provider": "openai",
      "model": "gpt-4",
      "apiKey": "sk-your-api-key-here"
    }
  ]
}
```

### Supported Providers
Continue supports many AI providers:

- **OpenAI**: GPT-4, GPT-3.5
- **Anthropic**: Claude 3 (Opus, Sonnet, Haiku)
- **Local Models**: Ollama, LM Studio
- **Azure OpenAI**: Enterprise deployments
- **Google**: PaLM 2
- **And more**: See [Continue documentation](https://continue.dev/docs/reference/Model%20Providers/openai)

Example with Claude:
```json
{
  "models": [
    {
      "title": "Claude 3 Sonnet",
      "provider": "anthropic",
      "model": "claude-3-sonnet-20240229",
      "apiKey": "sk-ant-your-api-key-here"
    }
  ]
}
```

Example with Ollama (local):
```json
{
  "models": [
    {
      "title": "Llama 3",
      "provider": "ollama",
      "model": "llama3"
    }
  ]
}
```

## 🎯 Usage Examples

### Using Custom Commands

#### Create a New Request
1. Open Continue (Cmd/Ctrl + L)
2. Type `/bruno-request`
3. Describe your request: "Create a POST request to create a new product"

**Expected Output**:
```bru
meta {
  name: Create Product
  type: http
  seq: 1
}

post {
  url: {{baseUrl}}/api/products
  body: json
  auth: bearer
}

headers {
  content-type: application/json
}

auth:bearer {
  token: {{authToken}}
}

body:json {
  {
    "name": "{{productName}}",
    "price": {{productPrice}},
    "category": "{{productCategory}}"
  }
}

tests {
  test("Product created successfully", function() {
    expect(res.status).to.equal(201);
    expect(res.body).to.have.property("id");
  });
}
```

#### Add Tests
1. Open a .bru file
2. Type `/bruno-test`
3. Continue will add comprehensive tests

#### Create Request Chain
1. Type `/bruno-chain`
2. Describe the workflow: "Login, then get user profile, then update settings"

### Natural Language Prompts

You can also use natural language:
- "Create a Bruno request for user authentication"
- "Add error handling to this pre-request script"
- "Generate an environment file for staging"

## 💡 Pro Tips

### Keyboard Shortcuts
- **Cmd/Ctrl + L**: Open Continue chat
- **Cmd/Ctrl + I**: Inline edit
- **Cmd/Ctrl + Shift + R**: Regenerate response

### Better Prompts
- ✅ "Create a Bruno .bru file for OAuth2 token refresh"
- ✅ "Add tests to validate the response schema"
- ❌ "Make an API call"

### Context Awareness
Continue automatically includes:
- Currently open files
- Selected code
- Recent edits
- Project structure

Reference files explicitly:
- "Similar to login.bru, create logout.bru"
- "Use the same auth pattern as in get-user.bru"

## 🐛 Troubleshooting

### Commands Not Showing
1. **Check Config**: Ensure `.continue/config.json` exists
2. **Restart Editor**: Close and reopen your editor
3. **Verify Installation**: Check that Continue extension is installed and active

### API Key Issues
1. **Check Format**: Ensure API key is correctly formatted
2. **Verify Provider**: Match provider name with model
3. **Test Connection**: Try a simple prompt to verify connectivity

### Poor Bruno Suggestions
1. **Use Slash Commands**: Start with `/bruno-request` for better context
2. **Be Specific**: Mention "Bruno .bru format" explicitly
3. **Update Config**: Ensure you have the latest config.json

## 🔄 Team Collaboration

### Sharing Configuration
Commit the config (without API keys):

```bash
# Add config to .gitignore
echo ".continue/config.json" >> .gitignore

# Create a template
cp .continue/config.json .continue/config.template.json
# Remove API keys from template
git add .continue/config.template.json
git commit -m "Add Continue configuration template"
```

### Team Setup
1. Each team member copies the template
2. Adds their own API keys
3. Benefits from shared custom commands

## 🔗 Related Resources

- [Continue Documentation](https://continue.dev/docs)
- [Continue GitHub](https://github.com/continuedev/continue)
- [Bruno Documentation](https://docs.usebruno.com)
- [Model Providers](https://continue.dev/docs/reference/Model%20Providers/openai)

## 📝 Example Workflow

1. **Install Continue**: Add extension to your editor
2. **Configure API**: Set up your preferred AI provider
3. **Open Bruno Project**: Navigate to your collection
4. **Use Slash Commands**: Try `/bruno-request` to create a new file
5. **Iterate**: Use Continue's chat to refine and improve
6. **Test**: Run your requests in Bruno

With Continue configured for Bruno, you get a powerful, customizable AI assistant that understands Bruno's unique patterns and can help you build API collections faster.

