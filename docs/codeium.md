# Codeium Setup for Bruno

[Codeium](https://codeium.com) is a free AI-powered code completion and chat assistant that automatically discovers context files to provide better suggestions for your Bruno projects.

## 🚀 Quick Setup

### Automatic Installation
```bash
cd your-bruno-project
curl -fsSL https://raw.githubusercontent.com/bruno-collections/ai-assistant-prompts/main/install.sh | bash
```

### Manual Installation
1. Install Codeium:
   - **VS Code**: [Codeium Extension](https://marketplace.visualstudio.com/items?itemName=Codeium.codeium)
   - **JetBrains**: [Codeium Plugin](https://plugins.jetbrains.com/plugin/20540-codeium)
   - **Vim/Neovim**: [codeium.vim](https://github.com/Exafunction/codeium.vim)

2. Create the `.codeium` directory:
```bash
mkdir -p .codeium
```

3. Download the Bruno context:
```bash
curl -fsSL https://raw.githubusercontent.com/bruno-collections/ai-assistant-prompts/main/prompts/codeium/.codeium/context.md -o .codeium/context.md
```

4. Restart your editor to load the context

## ✨ Features

### Bruno-Aware Completions
- **Bru Syntax**: Understands .bru file structure
- **Variable Patterns**: Suggests `{{variableName}}` format
- **Authentication**: Knows Bruno's auth blocks
- **Testing**: Suggests Chai.js assertions
- **Scripting**: JavaScript API completions

### What Codeium Will Suggest
- ✅ Valid .bru file syntax and structure
- ✅ Proper meta blocks with name, type, seq
- ✅ Environment variable usage
- ✅ Authentication patterns (bearer, basic, API key)
- ✅ Pre-request and post-response scripts
- ✅ Comprehensive test assertions
- ✅ Dynamic variables ({{$randomEmail}}, {{$guid}}, etc.)

## 🎯 Usage Examples

### Code Completion

#### Creating Request Files
Start typing in a `.bru` file:

```bru
meta {
  name: Delete User
  type: http
  seq: 3
}

delete {
  url: {{baseUrl}}/users/{{userId}}
  body: none
  auth: bearer
}

auth:bearer {
  token: {{authToken}}
}

tests {
  test("User deleted successfully", function() {
    expect(res.status).to.equal(204);
  });
}
```

#### Environment Variables
In environment files:

```bru
vars {
  baseUrl: https://api.example.com
  apiVersion: v1
  timeout: 5000
}

vars:secret [
  authToken,
  apiKey,
  clientSecret
]
```

#### Pre-Request Scripts
Codeium suggests common patterns:

```bru
script:pre-request {
  // Set timestamp
  bru.setVar("timestamp", Date.now());
  
  // Generate random email
  bru.setVar("email", `user-${Date.now()}@example.com`);
  
  // Set request timeout
  req.setTimeout(10000);
}
```

### Chat Assistant

Use Codeium Chat for:
- "Create a Bruno request for user registration"
- "Add error handling to this script"
- "Generate tests for this API endpoint"
- "Create an environment file for production"

## 💡 Getting Better Suggestions

### File Naming
Use descriptive names:
- ✅ `create-user.bru`
- ✅ `get-user-profile.bru`
- ✅ `update-settings.bru`
- ❌ `request1.bru`
- ❌ `test.bru`

### Add Comments
Help Codeium understand context:

```bru
meta {
  name: OAuth2 Token Refresh
  type: http
  seq: 5
}

# Refresh the OAuth2 access token using the refresh token
post {
  url: {{authUrl}}/oauth/token
  body: json
  auth: none
}
```

### Sequential Development
Build requests step by step:
1. Start with meta block
2. Add request method and URL
3. Include headers
4. Add authentication
5. Include body (if needed)
6. Add tests
7. Add scripts (if needed)

## 🔧 Supported Editors

### VS Code
1. Install the [Codeium extension](https://marketplace.visualstudio.com/items?itemName=Codeium.codeium)
2. Place context file in `.codeium/context.md`
3. Codeium automatically reads the context
4. Use Cmd/Ctrl + I for chat

### JetBrains IDEs
1. Install the [Codeium plugin](https://plugins.jetbrains.com/plugin/20540-codeium)
2. Ensure context file is in `.codeium/context.md`
3. Restart the IDE
4. Access chat via Tools > Codeium

### Vim/Neovim
1. Install [codeium.vim](https://github.com/Exafunction/codeium.vim)
2. Context file will be automatically discovered
3. Use `:Codeium Chat` for chat interface

## 🐛 Troubleshooting

### Codeium Not Using Context
1. **Check File Location**: Ensure file is at `.codeium/context.md`
2. **Restart Editor**: Close and reopen your editor
3. **Verify Installation**: Check that Codeium is installed and authenticated
4. **Check Logs**: Look for errors in Codeium output panel

### Poor Suggestions
1. **Add Context**: Include more descriptive comments in files
2. **Use Bruno Terminology**: Mention "Bruno", ".bru", "environment variables"
3. **Reference Existing Files**: Keep similar requests open for context
4. **Update Context**: Ensure you have the latest context.md file

### Missing Bruno Features
1. **Update Context File**: Download the latest version
2. **Provide Examples**: Include example .bru files in your project
3. **Use Specific Names**: Name files with Bruno-specific patterns
4. **Clear Cache**: Try clearing Codeium's cache and restarting

## 🔄 Team Collaboration

### Repository Setup
Commit the context file to your repository:

```bash
git add .codeium/context.md
git commit -m "Add Codeium context for Bruno development"
git push
```

### Team Benefits
- **Consistent Suggestions**: All team members get Bruno-aware completions
- **Faster Onboarding**: New developers get immediate context
- **Best Practices**: Codeium suggests Bruno best practices
- **Free for Everyone**: Codeium is free for individual developers

## 📊 Measuring Effectiveness

### Good Indicators
- ✅ Codeium suggests .bru syntax instead of JSON
- ✅ Variable suggestions use `{{variableName}}` format
- ✅ Test suggestions use Chai.js assertions
- ✅ Authentication patterns match Bruno's auth blocks
- ✅ Script suggestions use bru.setVar(), bru.getVar()

### Improvement Areas
- ❌ Suggestions use JSON format for requests
- ❌ Missing environment variable patterns
- ❌ Generic test assertions instead of Bruno-specific
- ❌ Incorrect JavaScript API usage

## 💡 Pro Tips

### Keyboard Shortcuts (VS Code)
- **Tab**: Accept suggestion
- **Cmd/Ctrl + →**: Accept word
- **Cmd/Ctrl + I**: Open chat
- **Esc**: Dismiss suggestion

### Better Prompts
- ✅ "Create a Bruno .bru file for OAuth2 authentication"
- ✅ "Add comprehensive tests with Chai.js assertions"
- ❌ "Create an API request"
- ❌ "Add tests"

### Leverage Context
- Reference existing files: "Similar to login.bru, create signup.bru"
- Mention environment: "Use the production environment variables"
- Specify auth: "Use bearer token authentication"

## 🔗 Related Resources

- [Codeium Documentation](https://codeium.com/docs)
- [Codeium Blog](https://codeium.com/blog)
- [Bruno Documentation](https://docs.usebruno.com)
- [Bru Language Specification](https://github.com/usebruno/bruno/tree/main/packages/bruno-lang)

## 📝 Example Workflow

1. **Install Codeium**: Add extension to your editor
2. **Set Up Context**: Place `.codeium/context.md` in your project
3. **Open Bruno Project**: Navigate to your collection
4. **Start Typing**: Create a new .bru file and let Codeium suggest
5. **Use Chat**: Ask Codeium to generate complex requests
6. **Refine**: Iterate on suggestions to match your needs
7. **Test**: Run your requests in Bruno

With Codeium configured for Bruno, you get intelligent, context-aware code completions and chat assistance that understands Bruno's unique patterns - all for free!

