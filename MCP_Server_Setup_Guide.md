# Adding AWS Documentation MCP Server to Amazon Q

This guide documents the process of adding the AWS Documentation MCP (Model Context Protocol) server to Amazon Q for command line, based on a successful installation performed on macOS.

## Overview

The AWS Documentation MCP server provides Amazon Q with the ability to search, read, and get recommendations from official AWS documentation. This extends Q's capabilities with real-time access to AWS documentation.

## Prerequisites

- Amazon Q CLI installed and configured ([Installation Guide](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-getting-started-installing.html))
- Internet connection for downloading tools and accessing documentation
- macOS, Linux, or Windows (this guide covers all platforms)

## Step 1: Install UV Package Manager

First, install the UV package manager which will be used to run the MCP server (macOS and Linux):

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

If you don't have curl installed or have Windows, check other options here: https://docs.astral.sh/uv/getting-started/installation/

**Expected Output:**
```
downloading uv 0.7.19 aarch64-apple-darwin
no checksums to verify
installing to /Users/[username]/.local/bin
  uv
  uvx
everything's installed!
```

## Step 2: Navigate to Amazon Q Configuration Directory

Navigate to the Amazon Q configuration directory:

```bash
cd ~/.aws/amazonq
```

**Note:** If this directory doesn't exist, create it first:
```bash
mkdir -p ~/.aws/amazonq
```

## Step 3: Create MCP Configuration File

Create the `mcp.json` configuration file:

```bash
nano mcp.json
```

Add the following configuration:

```json
{
  "mcpServers": {
    "awslabs.aws-documentation-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR",
        "AWS_DOCUMENTATION_PARTITION": "aws"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

## Step 4: Verify Configuration

Verify the configuration file was created correctly:

```bash
ls
# Should show: cache  history  lspLog.log  mcp.json  personas  profiles

cat mcp.json
# Should display the JSON configuration
```

## Step 5: Start Amazon Q with MCP Server

Launch Amazon Q chat with the MCP server:

```bash
q chat
```

**Expected Output:**
```
✓ awslabsaws_documentation_mcp_server loaded in 1.88 s
✓ 1 of 1 mcp servers initialized.
```

## Configuration Details

### MCP Server Configuration Breakdown

- **Server Name**: `awslabs.aws-documentation-mcp-server`
- **Command**: `uvx` (UV package executor)
- **Arguments**: `["awslabs.aws-documentation-mcp-server@latest"]` (runs latest version)
- **Environment Variables**:
  - `FASTMCP_LOG_LEVEL`: Set to "ERROR" to minimize logging
  - `AWS_DOCUMENTATION_PARTITION`: Set to "aws" for standard AWS partition
- **Disabled**: `false` (server is enabled)
- **Auto Approve**: `[]` (empty array, manual approval required initially)

### Available Tools After Installation

Once the MCP server is loaded, Amazon Q gains access to these additional tools:

1. **awslabsaws_documentation_mcp_server___search_documentation**
   - Search AWS documentation using official AWS Documentation Search API
   - Useful for finding relevant documentation pages

2. **awslabsaws_documentation_mcp_server___read_documentation**
   - Fetch and convert AWS documentation pages to markdown format
   - Can handle long documents with pagination support

3. **awslabsaws_documentation_mcp_server___recommend**
   - Get content recommendations for AWS documentation pages
   - Provides related, popular, new, and journey-based recommendations

## Usage Tips

### First-Time Tool Usage
- When first using MCP tools, Q will ask for permission
- Use 't' to trust and always allow the tool for the session
- Use 'y' for one-time approval or 'n' to deny

### Tool Trust Management
```
Allow this action? Use 't' to trust (always allow) this tool for the session. [y/n/t]:
```

### Example Usage
After setup, you can ask Amazon Q to:
- "Search for EKS best practices documentation"
- "Read the latest Lambda documentation"
- "Find recommendations related to S3 security"

## Troubleshooting

### Common Issues

1. **MCP Server Not Loading**
   - Verify `mcp.json` syntax is correct
   - Check that UV is properly installed and in PATH
   - Ensure internet connectivity for downloading the server

2. **Permission Errors**
   - Make sure the `~/.aws/amazonq` directory has proper permissions
   - Verify UV installation completed successfully

3. **Tool Not Available**
   - Restart Amazon Q chat session
   - Check MCP server initialization messages on startup

### Verification Commands

```bash
# Check if UV is installed
which uvx

# Verify MCP configuration
cat ~/.aws/amazonq/mcp.json

# Check Amazon Q directory structure
ls -la ~/.aws/amazonq/
```

## Benefits of This Setup

1. **Real-time Documentation Access**: Get up-to-date AWS documentation without leaving the chat
2. **Enhanced Accuracy**: Responses are backed by official AWS documentation
3. **Comprehensive Coverage**: Access to the entire AWS documentation library
4. **Citation Support**: Proper source attribution for all information
5. **Discovery Features**: Find related and recommended documentation

## Security Considerations

- The MCP server only accesses public AWS documentation
- No AWS credentials or sensitive information is transmitted
- All communication is over HTTPS
- The server runs locally and doesn't store personal data

## Maintenance

- The server automatically uses the latest version (`@latest`)
- No manual updates required
- Configuration persists across Amazon Q sessions
- Can be disabled by setting `"disabled": true` in the configuration

## Conclusion

This MCP server integration significantly enhances Amazon Q's ability to provide accurate, up-to-date AWS guidance by giving it direct access to official AWS documentation. The setup is straightforward and provides immediate value for AWS-related queries and best practices research.
