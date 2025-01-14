# GitHub PR MCP Server

An enhanced GitHub Pull Request management server built on the Model Context Protocol (MCP). This server extends the base @modelcontextprotocol/server-github functionality with rich PR creation features.

## Features

- 🔄 Complete Base Server Compatibility
  - Create PRs with required fields (owner, repo, title, head, base)
  - Optional fields support (body, draft, maintainer_can_modify)
  - List and update PRs with matching schemas

- 🚀 Enhanced PR Creation
  - Structured PR templates with sections:
    - Overview
    - Key Changes
    - Code Highlights
    - Testing Details
  - Checklist items for PR quality
  - Media attachments support
  - Labels, reviewers, and assignees management
  - Issue linking and tagging

## Prerequisites

- Node.js 18+
- GitHub Personal Access Token with repo scope
- Pull request template file at `.github/pull_request_template.md` (example below)

### Example PR Template

Create `.github/pull_request_template.md` with sections that match the enhanced PR features:

```markdown
## Overview

Summarize the purpose and scope of this PR.

---

## Key Changes

List the key changes made in this PR.

---

## Code Highlights

Identify important code sections that reviewers should focus on.

---

## Testing

Outline testing approach and results.

---

## Links

Include relevant links and references.

---

## Additional Notes

Provide any extra context or notes for reviewers.

---

### Issue Tagging

Link related issues here.

---
```

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/cline-github-mcp.git
   cd cline-github-mcp
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Build the project:
   ```bash
   npm run build
   ```

## Environment Variables

Create a `.env` file in the root directory:

```env
# Required for GitHub API access
GITHUB_TOKEN=your_github_personal_access_token
```

## Usage

### Running the Server

Start the MCP server:
```bash
npm start
```

For development:
```bash
npm run dev
```

### MCP Server Configuration

This enhanced GitHub PR server can run alongside the base GitHub server in various environments. Here's how to configure each:

#### Claude Desktop

1. Open Claude Desktop settings:
   - Mac: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - Windows: `%APPDATA%/Claude/claude_desktop_config.json`
   - Linux: `~/.config/Claude/claude_desktop_config.json`

2. Add both servers to the configuration:
   ```json
   {
     "mcpServers": {
       "github": {
         "command": "npx",
         "args": ["-y", "@modelcontextprotocol/server-github"],
         "env": {
           "GITHUB_TOKEN": "your_github_personal_access_token"
         }
       },
       "github-pr": {
         "command": "node",
         "args": ["/path/to/cline-github-mcp/build/index.js"],
         "env": {
           "GITHUB_TOKEN": "your_github_personal_access_token"
         }
       }
     }
   }
   ```

#### VSCode (Cline Extension)

1. Open VSCode settings:
   - Mac: `~/Library/Application Support/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json`
   - Windows: `%APPDATA%/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json`
   - Linux: `~/.config/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json`

2. Add both servers to the configuration:
   ```json
   {
     "mcpServers": {
       "github": {
         "command": "npx",
         "args": ["-y", "@modelcontextprotocol/server-github"],
         "env": {
           "GITHUB_TOKEN": "your_github_personal_access_token"
         }
       },
       "github-pr": {
         "command": "node",
         "args": ["/path/to/cline-github-mcp/build/index.js"],
         "env": {
           "GITHUB_TOKEN": "your_github_personal_access_token"
         }
       }
     }
   }
   ```

#### VSCode (Roo Cline Extension)

1. Open Roo Cline settings:
   - Mac: `~/Library/Application Support/Code/User/globalStorage/rooveterinaryinc.roo-cline/settings/cline_mcp_settings.json`
   - Windows: `%APPDATA%/Code/User/globalStorage/rooveterinaryinc.roo-cline/settings/cline_mcp_settings.json`
   - Linux: `~/.config/Code/User/globalStorage/rooveterinaryinc.roo-cline/settings/cline_mcp_settings.json`

2. Add both servers to the configuration:
   ```json
   {
     "mcpServers": {
       "github": {
         "command": "npx",
         "args": ["-y", "@modelcontextprotocol/server-github"],
         "env": {
           "GITHUB_TOKEN": "your_github_personal_access_token"
         }
       },
       "github-pr": {
         "command": "node",
         "args": ["/path/to/cline-github-mcp/build/index.js"],
         "env": {
           "GITHUB_TOKEN": "your_github_personal_access_token"
         }
       }
     }
   }
   ```

3. Restart your application after making changes

Note: Both servers can run simultaneously in all environments. The base `github` server provides standard GitHub operations, while the `github-pr` server adds enhanced PR creation features. You can use either server based on your needs - use the base server for simple operations or the enhanced server when you need rich PR features.

### Available Tools

#### create_pull_request
Create a new pull request with enhanced features
```typescript
{
  // Required fields (base compatibility)
  owner: string;
  repo: string;
  title: string;
  head: string;
  base: string;

  // Optional fields (base compatibility)
  body?: string;
  draft?: boolean;
  maintainer_can_modify?: boolean;

  // Enhanced features
  overview?: string;
  keyChanges?: string[];
  codeHighlights?: string[];
  testing?: string[];
  links?: Array<{ title: string; url: string }>;
  additionalNotes?: string;
  issueIds?: string[];
  checklist?: {
    adhereToConventions?: boolean;
    testsIncluded?: boolean;
    documentationUpdated?: boolean;
    changesVerified?: boolean;
    screenshotsAttached?: boolean;
  };
  attachments?: Array<{
    type: "image" | "video" | "diagram";
    alt: string;
    url: string;
    width?: number;
  }>;
  labels?: string[];
  reviewers?: string[];
  assignees?: string[];
}
```

#### list_pull_requests
List pull requests in a repository
```typescript
{
  owner: string;    // Required
  repo: string;     // Required
  state?: "open" | "closed" | "all";
}
```

#### update_pull_request
Update an existing pull request
```typescript
{
  owner: string;       // Required
  repo: string;       // Required
  pull_number: number; // Required
  title?: string;
  body?: string;
  state?: "open" | "closed";
}
```

## Development

### Directory Structure

```
src/
├── index.ts           # Main MCP server implementation
├── api/              # API routes and middleware
├── config/           # Configuration
└── types/            # TypeScript type definitions
```

### Adding New Features

1. Define types in `src/types/index.ts`
2. Update server implementation in `src/index.ts`
3. Add new tool handlers in the GitHubServer class
4. Update documentation

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

MIT License - see LICENSE file for details
