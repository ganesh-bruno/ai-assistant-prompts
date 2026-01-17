# VS Code AI Extensions Setup for Bruno

VS Code AI extensions (like Cline, Roo Code, Aider, and others) can automatically discover instruction files to provide better assistance for your Bruno projects.

## 🚀 Quick Setup

### Automatic Installation
```bash
cd your-bruno-project
curl -fsSL https://raw.githubusercontent.com/bruno-collections/bruno-ai-assistant-prompts/main/install.sh | bash
```

### Manual Installation
1. Create the `.vscode` directory if it doesn't exist:
```bash
mkdir -p .vscode
```

2. Download the AI instructions:
```bash
curl -fsSL https://raw.githubusercontent.com/bruno-collections/bruno-ai-assistant-prompts/main/prompts/vscode/.vscode/ai-instructions.md -o .vscode/ai-instructions.md
```

3. Restart VS Code to load the new instructions

## ✨ Compatible Extensions

### Supported AI Extensions
- **[Cline](https://marketplace.visualstudio.com/items?itemName=saoudrizwan.claude-dev)** - Claude-powered coding assistant
- **[Roo Code](https://marketplace.visualstudio.com/items?itemName=RooVeterinaryInc.roo-cline)** - AI pair programmer
- **[Aider](https://aider.chat/)** - AI pair programming in your terminal
- **[Cursor](https://cursor.sh/)** - AI-first code editor (also reads `.cursorrules`)
- **Other extensions** that follow the `.vscode/ai-instructions.md` convention

## 🎯 Features

### Bruno-Specific Understanding
- **Bru File Format**: Complete syntax and structure understanding
- **JavaScript API**: req, res, bru objects with all methods
- **Dynamic Variables**: {{$randomEmail}}, {{$guid}}, {{$timestamp}}, etc.
- **Testing**: Chai.js assertions and test patterns
- **Scripting**: Pre-request and post-response script examples

### What AI Assistants Will Help With
- ✅ Generate valid .bru files with proper syntax
- ✅ Create environment files with variables and secrets
- ✅ Write pre-request and post-response scripts
- ✅ Add comprehensive test assertions
- ✅ Implement authentication patterns
- ✅ Chain requests for workflows
- ✅ Manage cookies and sessions

## 💡 Usage Examples

### Creating a New API Request
**Prompt**: "Create a new .bru file for updating a user profile"

**Expected Output**:
```bru
meta {
  name: Update User Profile
  type: http
  seq: 2
}

put {
  url: {{baseUrl}}/users/{{userId}}
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
    "name": "{{userName}}",
    "email": "{{userEmail}}"
  }
}

tests {
  test("Profile updated successfully", function() {
    expect(res.status).to.equal(200);
    expect(res.body).to.have.property("id");
    expect(res.body.name).to.equal(bru.getVar("userName"));
  });
}
```

### Adding Request Chaining
**Prompt**: "Add a post-response script to extract the user ID and chain to the next request"

**Expected Output**:
```bru
script:post-response {
  if (res.status === 201) {
    // Extract user ID from response
    const userId = res.body.id;
    bru.setVar("userId", userId);
    
    // Chain to the next request
    bru.setNextRequest("Get User Profile");
  }
}
```

### Environment Setup
**Prompt**: "Create a production environment file"

**Expected Output**:
```bru
vars {
  baseUrl: https://api.production.example.com
  apiVersion: v2
  timeout: 10000
}

vars:secret [
  authToken,
  apiKey,
  clientSecret
]
```

## 🔧 Configuration

### Project-Specific Instructions
You can extend the `.vscode/ai-instructions.md` file with project-specific guidance:

```markdown
## Project-Specific Rules
- This API uses OAuth2 with PKCE flow
- All requests must include X-API-Version header
- Rate limiting: 100 requests per minute
- Use semantic versioning for API versions
```

### Team Collaboration
Commit the instruction file to your repository:

```bash
git add .vscode/ai-instructions.md
git commit -m "Add VS Code AI instructions for Bruno development"
```

## 🐛 Troubleshooting

### AI Not Using Instructions
1. **Check File Location**: Ensure file is at `.vscode/ai-instructions.md`
2. **Restart VS Code**: Close and reopen VS Code
3. **Check Extension**: Verify your AI extension supports instruction files
4. **File Permissions**: Ensure the file is readable

### Poor Suggestions
1. **Be Specific**: Mention "Bruno .bru format" in your prompts
2. **Provide Context**: Reference existing files in your collection
3. **Use Examples**: Point to similar requests as templates
4. **Update Instructions**: Ensure you have the latest version

### Extension-Specific Issues
1. **Cline**: Check the extension settings for custom instruction paths
2. **Roo Code**: Verify the extension is activated
3. **Aider**: Use `--read .vscode/ai-instructions.md` flag
4. **Other Extensions**: Consult extension documentation

## 💡 Pro Tips

### Better Prompts
- ✅ "Create a Bruno .bru file for user authentication with OAuth2"
- ❌ "Create an API request"

### Leverage Context
- Reference existing files: "Similar to create-user.bru, create delete-user.bru"
- Mention environment: "Use the staging environment variables"
- Specify auth: "Use bearer token authentication"

### Iterative Development
1. Start with basic request structure
2. Add authentication and headers
3. Include request body (if needed)
4. Add comprehensive tests
5. Include error handling scripts
6. Add request chaining (if needed)

## 🔗 Related Resources

- [Bruno Documentation](https://docs.usebruno.com)
- [Bru Language Specification](https://github.com/usebruno/bruno/tree/main/packages/bruno-lang)
- [VS Code Extension Marketplace](https://marketplace.visualstudio.com/vscode)

## 📝 Example Workflow

1. **Open Bruno Project**: Open your Bruno collection in VS Code
2. **Activate AI Assistant**: Start your preferred AI extension
3. **Create New Request**: Ask AI to create a new .bru file
4. **Add Authentication**: Request authentication setup
5. **Include Tests**: Add comprehensive test coverage
6. **Environment Variables**: Set up environment-specific values
7. **Test and Refine**: Run the request and iterate

With the Bruno-specific instructions in place, VS Code AI extensions become powerful assistants for API development, understanding the unique requirements of Bruno's file-based, Git-collaborative approach.

