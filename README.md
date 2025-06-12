![image](https://github.com/user-attachments/assets/8d7f3f88-27d4-43b1-bb14-8df289c1050b)

# MCP Setup Guide for the Agentic Engineering Trifecta made with Claude Code
*Model Context Protocol Integration for Claude, Roo Code SPARC, and GitHub Copilot*

## Table of Contents
1. [MCP Overview](#mcp-overview)
2. [Prerequisites](#prerequisites)
3. [Core MCP Setup](#core-mcp-setup)
4. [Trifecta Integration](#trifecta-integration)
5. [Essential MCP Servers](#essential-mcp-servers)
6. [Configuration Examples](#configuration-examples)
7. [Troubleshooting](#troubleshooting)
8. [Advanced Workflows](#advanced-workflows)

---

## MCP Overview

**Model Context Protocol (MCP)** is the secret sauce that supercharges your Agentic Engineering Trifecta. It allows your AI tools to securely connect to external services, databases, APIs, and cloud resources - transforming them from isolated assistants into connected intelligence networks.

### What MCP Enables:
- **Direct database queries** without writing SQL
- **Cloud deployment** through natural language
- **API integration** with external services
- **File system operations** across projects
- **Real-time data access** from live systems

### MCP in the Trifecta:
- **Claude**: Uses MCP for strategic data analysis and external research
- **Roo Code SPARC**: Leverages MCP for database design, deployment, and integration
- **GitHub Copilot**: Enhanced with MCP capabilities for cloud operations

---

## Prerequisites

### Required Tools:
- Node.js 18+ installed
- VS Code with Roo Code extension
- Claude Desktop app (for local MCP servers)
- GitHub Copilot subscription
- Active cloud accounts (AWS, Supabase, etc.)

### Recommended Setup:
```bash
# Verify Node.js version
node --version  # Should be 18+

# Install global MCP tools
npm install -g @anthropic-ai/mcp-client
npm install -g @modelcontextprotocol/server-filesystem
```

---

## Core MCP Setup

### Step 1: Claude Desktop MCP Configuration

Create Claude Desktop configuration file:

**macOS:**
```bash
# Create config directory
mkdir -p ~/Library/Application\ Support/Claude/

# Create MCP config
cat > ~/Library/Application\ Support/Claude/claude_desktop_config.json << 'EOF'
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/your/projects"],
      "env": {}
    },
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "${env:BRAVE_API_KEY}"
      }
    }
  }
}
EOF
```

**Windows:**
```bash
# Create config directory
mkdir "%APPDATA%\Claude\"

# Create config file at:
# %APPDATA%\Claude\claude_desktop_config.json
```

### Step 2: Environment Variables Setup

Create comprehensive `.env` file:

```bash
# Create global .env for MCP credentials
cat > ~/.mcp_env << 'EOF'
# Search and Research
BRAVE_API_KEY=your_brave_api_key
SERP_API_KEY=your_serp_api_key

# Cloud Providers
AWS_ACCESS_KEY_ID=your_aws_key
AWS_SECRET_ACCESS_KEY=your_aws_secret
AWS_DEFAULT_REGION=us-east-1

# Databases
SUPABASE_ACCESS_TOKEN=your_supabase_token
SUPABASE_PROJECT_ID=your_project_id
DATABASE_URL=postgresql://user:pass@host:port/db

# APIs
GITHUB_TOKEN=your_github_token
OPENAI_API_KEY=your_openai_key
ANTHROPIC_API_KEY=your_anthropic_key

# Communication
SLACK_BOT_TOKEN=your_slack_token
DISCORD_BOT_TOKEN=your_discord_token

# Development Tools
VERCEL_TOKEN=your_vercel_token
NETLIFY_TOKEN=your_netlify_token
EOF

# Source the environment
echo "source ~/.mcp_env" >> ~/.bashrc
echo "source ~/.mcp_env" >> ~/.zshrc
```

### Step 3: Roo Code SPARC MCP Integration

In your SPARC project directory:

```bash
# Navigate to your SPARC project
cd your-sparc-project

# Configure MCP for Roo Code
npx create-sparc configure-mcp

# Or manually create .roo/mcp.json
cat > .roo/mcp.json << 'EOF'
{
  "mcpServers": {
    "supabase": {
      "command": "npx",
      "args": ["-y", "@supabase/mcp-server-supabase", "--access-token", "${env:SUPABASE_ACCESS_TOKEN}"],
      "alwaysAllow": ["list_tables", "execute_sql", "get_project"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${env:GITHUB_TOKEN}"
      },
      "alwaysAllow": ["list_repositories", "get_file", "create_issue"]
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "."],
      "alwaysAllow": ["read_file", "write_file", "list_directory"]
    }
  }
}
EOF
```

---

## Trifecta Integration

### Integration Pattern 1: Shared MCP Infrastructure

```mermaid
graph TB
    subgraph "MCP Layer"
        A[Supabase MCP] 
        B[GitHub MCP]
        C[AWS MCP]
        D[Filesystem MCP]
    end
    
    subgraph "AI Tools"
        E[Claude Desktop]
        F[Roo Code SPARC]
        G[GitHub Copilot]
    end
    
    A --> E
    A --> F
    B --> E
    B --> F
    B --> G
    C --> E
    C --> F
    D --> E
    D --> F
```

### Integration Pattern 2: Tool-Specific MCP Servers

```json
{
  "claude_servers": ["research", "analysis", "filesystem"],
  "roo_sparc_servers": ["database", "deployment", "github"],
  "copilot_servers": ["github", "cloud", "testing"]
}
```

---

## Essential MCP Servers

### 1. Database Operations

#### Supabase MCP Server
```bash
# Install Supabase MCP
npm install -g @supabase/mcp-server-supabase

# Test connection
npx @supabase/mcp-server-supabase --access-token $SUPABASE_ACCESS_TOKEN
```

**Configuration:**
```json
{
  "supabase": {
    "command": "npx",
    "args": ["-y", "@supabase/mcp-server-supabase"],
    "env": {
      "SUPABASE_ACCESS_TOKEN": "${env:SUPABASE_ACCESS_TOKEN}",
      "SUPABASE_PROJECT_ID": "${env:SUPABASE_PROJECT_ID}"
    },
    "alwaysAllow": [
      "list_tables",
      "execute_sql", 
      "get_project",
      "list_organizations"
    ]
  }
}
```

#### PostgreSQL MCP Server
```bash
# Install PostgreSQL MCP
npm install -g @modelcontextprotocol/server-postgres

# Configuration
{
  "postgres": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-postgres"],
    "env": {
      "DATABASE_URL": "${env:DATABASE_URL}"
    }
  }
}
```

### 2. Cloud Operations

#### AWS MCP Server
```bash
# Install AWS MCP
npm install -g @modelcontextprotocol/server-aws

# Configuration
{
  "aws": {
    "command": "npx", 
    "args": ["-y", "@modelcontextprotocol/server-aws"],
    "env": {
      "AWS_ACCESS_KEY_ID": "${env:AWS_ACCESS_KEY_ID}",
      "AWS_SECRET_ACCESS_KEY": "${env:AWS_SECRET_ACCESS_KEY}",
      "AWS_DEFAULT_REGION": "${env:AWS_DEFAULT_REGION}"
    }
  }
}
```

### 3. Version Control

#### GitHub MCP Server
```bash
# Install GitHub MCP
npm install -g @modelcontextprotocol/server-github

# Configuration
{
  "github": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-github"],
    "env": {
      "GITHUB_TOKEN": "${env:GITHUB_TOKEN}"
    },
    "alwaysAllow": [
      "list_repositories",
      "get_file",
      "create_issue",
      "create_pull_request"
    ]
  }
}
```

### 4. Search and Research

#### Brave Search MCP
```bash
# Install Brave Search MCP
npm install -g @modelcontextprotocol/server-brave-search

# Configuration
{
  "brave-search": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-brave-search"],
    "env": {
      "BRAVE_API_KEY": "${env:BRAVE_API_KEY}"
    }
  }
}
```

### 5. Communication

#### Slack MCP Server
```bash
# Install Slack MCP
npm install -g @modelcontextprotocol/server-slack

# Configuration
{
  "slack": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-slack"],
    "env": {
      "SLACK_BOT_TOKEN": "${env:SLACK_BOT_TOKEN}"
    }
  }
}
```

---

## Configuration Examples

### Complete Claude Desktop Config

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/Users/yourname/projects"],
      "env": {}
    },
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "${env:BRAVE_API_KEY}"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${env:GITHUB_TOKEN}"
      }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "${env:DATABASE_URL}"
      }
    }
  }
}
```

### Complete Roo Code SPARC Config

```json
{
  "mcpServers": {
    "supabase": {
      "command": "npx",
      "args": ["-y", "@supabase/mcp-server-supabase"],
      "env": {
        "SUPABASE_ACCESS_TOKEN": "${env:SUPABASE_ACCESS_TOKEN}",
        "SUPABASE_PROJECT_ID": "${env:SUPABASE_PROJECT_ID}"
      },
      "alwaysAllow": [
        "list_tables",
        "execute_sql",
        "get_project",
        "apply_migration",
        "list_organizations"
      ]
    },
    "github": {
      "command": "npx", 
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${env:GITHUB_TOKEN}"
      },
      "alwaysAllow": [
        "list_repositories",
        "get_file", 
        "create_issue",
        "create_pull_request",
        "get_repository"
      ]
    },
    "aws": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-aws"],
      "env": {
        "AWS_ACCESS_KEY_ID": "${env:AWS_ACCESS_KEY_ID}",
        "AWS_SECRET_ACCESS_KEY": "${env:AWS_SECRET_ACCESS_KEY}",
        "AWS_DEFAULT_REGION": "${env:AWS_DEFAULT_REGION}"
      },
      "alwaysAllow": [
        "list_s3_buckets",
        "upload_to_s3",
        "list_ec2_instances"
      ]
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "."],
      "alwaysAllow": [
        "read_file",
        "write_file", 
        "list_directory",
        "create_directory"
      ]
    }
  }
}
```

### Project-Specific Environment Setup

```bash
# Create project-specific MCP environment
cat > .env.mcp << 'EOF'
# Project: My Trifecta App
# Database
SUPABASE_PROJECT_ID=abc123
DATABASE_URL=postgresql://postgres:password@localhost:5432/myapp

# GitHub
GITHUB_REPO_OWNER=myusername
GITHUB_REPO_NAME=my-trifecta-app

# Deployment
VERCEL_PROJECT_ID=prj_xyz789
AWS_S3_BUCKET=my-app-bucket

# APIs
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
EOF

# Load project environment
source .env.mcp
```

---

## Troubleshooting

### Common Issues

#### 1. MCP Server Not Starting
```bash
# Debug MCP server
npx @modelcontextprotocol/server-filesystem --debug

# Check logs
tail -f ~/.claude/logs/mcp.log
```

#### 2. Environment Variables Not Loading
```bash
# Test environment variables
echo $SUPABASE_ACCESS_TOKEN

# Reload environment
source ~/.mcp_env
source .env.mcp
```

#### 3. Permission Errors
```bash
# Fix file permissions
chmod +x ~/.mcp_env
chmod 600 ~/.mcp_env  # Secure credentials

# Verify MCP server permissions
ls -la ~/.claude/
```

#### 4. Connection Timeouts
```json
{
  "supabase": {
    "command": "npx",
    "args": ["-y", "@supabase/mcp-server-supabase", "--timeout", "30000"],
    "env": {
      "SUPABASE_ACCESS_TOKEN": "${env:SUPABASE_ACCESS_TOKEN}"
    }
  }
}
```

### Debugging Commands

```bash
# Test MCP server connectivity
npx @modelcontextprotocol/inspector

# Validate MCP configuration
npx @anthropic-ai/mcp-client validate config.json

# Check server logs
tail -f ~/.claude/logs/claude_desktop.log

# Test specific server
npx @supabase/mcp-server-supabase --test
```

---

## Advanced Workflows

### Workflow 1: End-to-End Development with MCP

#### Step 1: Claude Strategic Planning with Research
```
# In Claude with Brave Search MCP:
"Research current best practices for building scalable Node.js APIs. Include recent performance benchmarks and security considerations."
```

#### Step 2: SPARC Database Design with Supabase MCP
```
# In Roo Code SPARC Supabase Admin mode:
"Design a database schema for a todo API with user authentication. Create the tables, RLS policies, and initial data using Supabase MCP."
```

#### Step 3: GitHub Integration with MCP
```
# In SPARC GitHub Integration mode:
"Create a new repository, set up CI/CD workflows, and configure branch protection rules using GitHub MCP."
```

### Workflow 2: Real-Time Monitoring and Deployment

#### Continuous Deployment Pipeline:
```json
{
  "pipeline": [
    "SPARC Code → GitHub MCP → Create PR",
    "Copilot Agent → AWS MCP → Deploy to staging", 
    "SPARC Testing → Supabase MCP → Run integration tests",
    "Claude Analysis → Slack MCP → Send status update"
  ]
}
```

### Workflow 3: Data-Driven Development

#### Analytics and Insights:
```
1. Claude + Database MCP: Analyze user behavior data
2. SPARC + Research MCP: Research feature optimization strategies  
3. Copilot + Cloud MCP: Implement A/B testing infrastructure
```

---

## Security Best Practices

### 1. Credential Management
```bash
# Use credential managers
# macOS Keychain
security add-generic-password -a "mcp" -s "supabase_token" -w "your_token"

# Linux pass
pass insert mcp/supabase_token

# Windows Credential Manager
cmdkey /add:mcp_supabase /user:token /pass:your_token
```

### 2. Network Security
```json
{
  "security": {
    "allowed_hosts": ["supabase.co", "github.com", "aws.amazon.com"],
    "ssl_verify": true,
    "timeout": 30000
  }
}
```

### 3. Access Control
```json
{
  "permissions": {
    "read_only": ["list_tables", "get_file"],
    "write_limited": ["create_issue", "upload_to_s3"],
    "admin_only": ["delete_table", "delete_repository"]
  }
}
```

---

## Performance Optimization

### 1. Connection Pooling
```json
{
  "postgres": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-postgres", "--pool-size", "10"],
    "env": {
      "DATABASE_URL": "${env:DATABASE_URL}"
    }
  }
}
```

### 2. Caching
```json
{
  "github": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-github", "--cache-ttl", "300"],
    "env": {
      "GITHUB_TOKEN": "${env:GITHUB_TOKEN}"
    }
  }
}
```

### 3. Request Batching
```javascript
// Batch multiple operations
const batch_operations = [
  { operation: "list_tables", server: "supabase" },
  { operation: "get_repository", server: "github" },
  { operation: "list_buckets", server: "aws" }
];
```

---

## Maintenance and Updates

### Regular Maintenance Tasks

```bash
# Update MCP servers
npm update -g @modelcontextprotocol/server-*
npm update -g @supabase/mcp-server-supabase

# Clean up logs
find ~/.claude/logs -name "*.log" -mtime +7 -delete

# Backup configurations
cp ~/.claude/claude_desktop_config.json ~/.claude/config_backup_$(date +%Y%m%d).json
```

### Health Checks

```bash
#!/bin/bash
# MCP Health Check Script

echo "🔍 MCP Health Check"
echo "==================="

# Test each MCP server
servers=("supabase" "github" "aws" "filesystem")

for server in "${servers[@]}"; do
    echo "Testing $server..."
    if npx @modelcontextprotocol/server-$server --test; then
        echo "✅ $server: OK"
    else
        echo "❌ $server: FAILED"
    fi
done

echo "Health check complete!"
```

---

## Quick Start Checklist

### Essential Setup (5 minutes):
- [ ] Install Node.js 18+
- [ ] Create `.env` with basic credentials
- [ ] Configure Claude Desktop MCP
- [ ] Test filesystem MCP connection

### Trifecta Integration (15 minutes):
- [ ] Set up Roo Code SPARC MCP config
- [ ] Configure Supabase MCP server
- [ ] Set up GitHub MCP integration
- [ ] Test end-to-end workflow

### Production Ready (30 minutes):
- [ ] Implement security best practices
- [ ] Set up monitoring and logging
- [ ] Configure backup and recovery
- [ ] Document team workflows

---

## Conclusion

MCP transforms your Agentic Engineering Trifecta from three separate AI tools into a unified, connected intelligence network. With proper MCP setup, your AI assistants can:

- **Query live databases** in real-time
- **Deploy to cloud platforms** autonomously  
- **Integrate with external APIs** seamlessly
- **Collaborate across tools** with shared context

The result? AI-powered development that feels like magic but runs on solid infrastructure.

### Next Steps:
1. Complete the Quick Start Checklist
2. Implement your first MCP-powered workflow
3. Gradually add more MCP servers as needed
4. Scale to team-wide adoption

**Welcome to the connected AI future!** 🚀

---

*Last Updated: June 2025*
*Version: 1.0*
*Companion to: The Agentic Engineering Trifecta Manual*

