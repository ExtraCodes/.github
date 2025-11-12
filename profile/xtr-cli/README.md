<div align="center">

# 💻 XTR CLI

**Command-line tool for ExtraCodes platform**

[![npm](https://img.shields.io/badge/npm-xtr--cli-red?style=for-the-badge&logo=npm)](https://www.npmjs.com/package/xtr-cli)
[![Website](https://img.shields.io/badge/Website-extra.codes-blue?style=for-the-badge)](https://extra.codes)
[![Discord](https://img.shields.io/badge/Discord-Join%20Server-5865F2?style=for-the-badge&logo=discord)](https://discord.gg/zU4DtNUCnf)

*Manage your ExtraCodes repositories from the command line with familiar commands*

</div>

---

## 📖 About

XTR CLI is a powerful command-line tool that lets you manage your ExtraCodes repositories directly from your terminal. With familiar commands, you can push, pull, clone, and manage your code snippets without leaving the command line.

### ✨ Key Features

- **🔐 Secure Authentication** - Account tokens and SSH key support
- **📦 Commands** - Familiar `add`, `push`, `pull`, `clone` commands
- **🌐 Remote Management** - Manage multiple remotes
- **📝 File Staging** - Stage files before pushing
- **🔄 Version Control** - Track changes with commit messages
- **📚 Library Support** - Use programmatically in Node.js applications
- **⚡ Fast & Reliable** - Built with TypeScript for performance

---

## 🚀 Quick Start

### Installation

```bash
npm install -g xtr-cli
```

After installation, you can use `xtr` from anywhere:

```bash
xtr --help
```

### First Steps

1. **Login to your account:**
   ```bash
   xtr login
   ```
   Enter your username and account token (token input is hidden for security).

2. **Get your account token:**
   - Go to [extra.codes](https://extra.codes) and navigate to `/profile/[username]/settings`
   - Click on the **"Account Token"** tab
   - Click **"Generate Token"** and copy your token
   - Use this token when running `xtr login`

3. **Initialize a repository:**
   ```bash
   xtr init
   ```

4. **Stage files:**
   ```bash
   xtr add .
   ```

5. **Push to create a new code:**
   ```bash
   xtr push --name "My Project" --readme "Project description"
   ```

---

## 📚 Commands

### `xtr login`

Login to your ExtraCodes account using your username and account token.

```bash
xtr login
```

This command will:
- Prompt for your username
- Prompt for your account token (input is hidden for security)
- Verify your credentials with the API
- Save credentials to `.xtr/user.json` for future use

**Getting your account token:**
1. Go to `/profile/[username]/settings` on [extra.codes](https://extra.codes)
2. Click on the **"Account Token"** tab
3. Click **"Generate Token"** and copy it
4. Use it when prompted by `xtr login`

**Note:** You need to run `xtr login` before using other commands like `push`, `pull`, `clone`, or `remote`.

---

### `xtr init`

Initialize an XTR repository in the current directory.

```bash
xtr init
```

This creates:
- `.xtr/` directory with repository configuration
- `.xtrignore` file (if it doesn't exist) with default ignore patterns

**Example:**
```bash
cd my-project
xtr init
```

---

### `xtr add`

Stage files for commit.

```bash
xtr add [path]
```

**Arguments:**
- `path` (optional): File or directory to add. Defaults to `.` (current directory)

**Examples:**
```bash
# Stage all files in current directory
xtr add .

# Stage a specific file
xtr add src/index.js

# Stage a directory
xtr add src/
```

**Note:** Files matching patterns in `.xtrignore` will be automatically excluded.

---

### `xtr push`

Push staged files to ExtraCodes. Creates a new code or updates an existing one.

```bash
xtr push [options]
```

**Options:**
- `-m, --message <message>`: Commit message
- `-n, --name <name>`: Code name (required for new codes)
- `-r, --readme <readme>`: Code readme/description
- `-t, --tags <tags>`: Comma-separated tags
- `--code-id <id>`: Existing code ID to update

**Examples:**
```bash
# Create a new code
xtr push --name "My Awesome Project" --readme "A cool project" --tags "javascript,node"

# Update existing code (codeId from .xtr/config.json)
xtr push --message "Updated features"

# Update specific code
xtr push --code-id EC1234567890-abc123 --message "Bug fixes"
```

**Output:**
The command shows a preview of files being pushed with their sizes, and indicates:
- `create mode 100644 <file>` (green) - new files
- `modified <file>` (yellow) - modified files
- `unchanged <file>` (gray) - unchanged files

At the end, it shows a summary:
- `+ X new file(s)`
- `~ X modified file(s)`

---

### `xtr pull`

Pull files from an ExtraCodes URL or remote to the current directory.

```bash
xtr pull [url-or-remote]
```

**Arguments:**
- `url-or-remote` (optional): ExtraCodes URL or remote name. If omitted, uses the configured remote.

**Behavior:**
- If no argument provided:
  - If one remote exists: uses it automatically
  - If multiple remotes exist: prompts you to select one
  - If no remotes: shows error with instructions
- If URL provided: pulls from that URL
- If remote name provided: pulls from that remote

**Examples:**
```bash
# Pull from URL
xtr pull https://extra.codes/d/EC1234567890-abc123

# Pull using remote (if only one remote exists)
xtr pull

# Pull from specific remote
xtr pull origin
```

**Output:**
The command shows changes being pulled:
- `+ <file>` (green) - new files being added
- `~ <file>` (yellow) - modified files
- `- <file>` (red) - removed files

At the end, it shows a summary:
- `✓ Pulled X file(s) (X new, X modified, X removed)`
- Or `✓ Pulled X file(s) (no changes)` if nothing changed

---

### `xtr clone`

Clone a code repository from ExtraCodes to a new directory.

```bash
xtr clone <url> [options]
```

**Arguments:**
- `url`: ExtraCodes URL (e.g., `https://extra.codes/d/EC1234567890-abc123`)

**Options:**
- `-d, --dir <directory>`: Directory to clone into (defaults to code name)

**Examples:**
```bash
# Clone to default directory
xtr clone https://extra.codes/d/EC1234567890-abc123

# Clone to specific directory
xtr clone https://extra.codes/d/EC1234567890-abc123 --dir my-project
```

**Note:** This automatically initializes an XTR repository in the cloned directory with the remote configured as "origin".

---

### `xtr remote`

Manage remote URLs for your repository.

```bash
xtr remote [command] [name] [url]
```

**Commands:**
- `add <name> <url>`: Add a new remote
- `remove <name>`: Remove a remote
- `set-url <name> <url>`: Update remote URL
- `show` or `-v`: Show all remotes (default)

**Examples:**
```bash
# Show all remotes
xtr remote
xtr remote show

# Add a remote
xtr remote add origin https://extra.codes/d/EC1234567890-abc123

# Set default remote (creates/updates "origin")
xtr remote https://extra.codes/d/EC1234567890-abc123

# Remove a remote
xtr remote remove origin

# Update remote URL
xtr remote set-url origin https://extra.codes/d/EC9876543210-xyz789
```

---

## 🔐 Authentication

XTR uses account tokens for authentication. You need to run `xtr login` before using commands that require authentication.

### Account Token Authentication

Account tokens are the primary authentication method for the CLI.

#### Getting Your Account Token

1. Go to your profile settings: `/profile/[username]/settings` on [extra.codes](https://extra.codes)
2. Click on the **"Account Token"** tab
3. Click **"Generate Token"** (or **"Regenerate Token"** if you already have one)
4. Click the **copy button** to copy your token to the clipboard
5. Keep your token secure - treat it like a password!

#### Using Your Token

Run `xtr login` and enter your username and token when prompted:

```bash
xtr login
```

The token input is hidden for security. Your credentials are saved to `.xtr/user.json` and used automatically for all authenticated operations.

**Note:** If you regenerate your token, you'll need to run `xtr login` again with the new token.

### SSH Key Authentication (Optional)

SSH keys can be added to your profile for additional authentication options, but the primary method is account tokens via `xtr login`.

#### Adding SSH Keys

1. Go to your profile settings: `/profile/[username]/settings` on [extra.codes](https://extra.codes)
2. Click on the **"SSH Keys"** tab
3. Click **"Add SSH Key"**
4. Enter a description (optional) and paste your SSH public key
5. Click **"Add SSH Key"** to save

#### SSH Key Format

SSH keys must be in one of these formats:
- `ssh-rsa` (RSA keys)
- `ssh-ed25519` (Ed25519 keys)
- `ecdsa-sha2-nistp256/384/521` (ECDSA keys)
- `ssh-dss` (DSA keys)

**Example:**
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC... user@example.com
```

---

## ⚙️ Configuration

### Local Configuration (`.xtr/config.json`)

Each repository has a local configuration file that stores repository-specific settings:

```json
{
  "cloned": "https://extra.codes/d/EC1234567890-abc123",
  "remotes": {
    "origin": {
      "url": "https://extra.codes/d/EC1234567890-abc123",
      "codeId": "EC1234567890-abc123"
    },
    "upstream": {
      "url": "https://extra.codes/d/EC9876543210-xyz789",
      "codeId": "EC9876543210-xyz789"
    }
  }
}
```

**Fields:**
- `cloned`: The URL the repository was cloned from (if applicable)
- `remotes`: Object containing remote configurations
  - Each remote has a `name` (e.g., "origin", "upstream")
  - Each remote contains:
    - `url`: The ExtraCodes URL
    - `codeId`: The code ID extracted from the URL

### User Credentials (`.xtr/user.json`)

Your login credentials are stored in `.xtr/user.json`:

```json
{
  "username": "your-username",
  "token": "your-account-token"
}
```

This file is created when you run `xtr login`.

**Note:** API URLs are hardcoded in the package (`https://api.extra.codes` and `https://files.extra.codes`) and cannot be changed by users. For local development, you can override them using environment variables:
- `XTR_API_URL` - Override API URL (default: `https://api.extra.codes`)
- `XTR_FILES_API_URL` - Override Files API URL (default: `https://files.extra.codes`)

---

## 📄 `.xtrignore` File

Similar to `.gitignore`, the `.xtrignore` file specifies files and directories to exclude.

**Default patterns:**
```
.xtr/
.xtrignore
node_modules/
.git/
.gitignore
dist/
build/
.next/
*.log
.DS_Store
Thumbs.db
```

**Custom patterns:**
```
# Custom ignore patterns
*.tmp
.env
coverage/
```

---

## 💻 Using as a Library

XTR can be used programmatically in your Node.js applications.

### Installation

```bash
npm install xtr-cli
```

### Basic Usage

```javascript
const xtr = require('xtr-cli');
// or
import * as xtr from 'xtr-cli';
```

### Library API

#### Commands

```javascript
// Login (credentials saved to .xtr/user.json)
await xtr.loginCommand();

// Initialize a repository
await xtr.initCommand();

// Stage files
await xtr.addCommand('.');

// Push files
await xtr.pushCommand({
  name: 'My Project',
  readme: 'Project description',
  tags: 'javascript,node',
  message: 'Initial commit'
});

// Pull files
await xtr.pullCommand('https://extra.codes/d/EC1234567890-abc123');

// Clone repository
await xtr.cloneCommand('https://extra.codes/d/EC1234567890-abc123', {
  dir: 'my-project'
});

// Manage remotes
await xtr.remoteCommand('add', 'origin', 'https://extra.codes/d/EC1234567890-abc123');
```

#### Utilities

```javascript
// Get SSH keys
const sshKeys = await xtr.getUserSSHKeys();

// Verify SSH key format
const isValid = xtr.verifySSHKeyFormat('ssh-rsa AAAAB3...');

// Verify multiple SSH keys
const verifications = await xtr.verifySSHKeys(sshKeys);

// Get first valid SSH key
const validKey = await xtr.getValidSSHKey();

// Get SSH key fingerprint
const fingerprint = xtr.getSSHKeyFingerprint('ssh-rsa AAAAB3...');

// Extract code ID from URL
const codeId = xtr.extractCodeIdFromUrl('https://extra.codes/d/EC1234567890-abc123');

// Configuration
const config = await xtr.getConfig();
await xtr.setConfig({ remotes: { origin: { url: '...', codeId: '...' } } });

// User credentials
const credentials = await xtr.getUserCredentials();
await xtr.setUserCredentials({ username: 'user', token: 'token' });

// File operations
const files = await xtr.collectFiles('.', process.cwd());
const ignoreRules = await xtr.loadIgnoreRules(process.cwd());
const shouldIgnore = xtr.shouldIgnore(ignoreRules, 'node_modules/', process.cwd());

// API requests (authentication is handled automatically)
const response = await xtr.apiRequest('/api/v1/codes', {
  method: 'GET'
});

const filesResponse = await xtr.filesApiRequest('/api/v1/files/EC1234567890-abc123');
```

### TypeScript Support

XTR includes TypeScript definitions:

```typescript
import * as xtr from 'xtr-cli';
import type { ApiResponse, XtrConfig, SSHKey, SSHKeyVerification, UserCredentials } from 'xtr-cli';

const config: XtrConfig = await xtr.getConfig();
const response: ApiResponse<{ codes: any[] }> = await xtr.apiRequest('/api/v1/codes');
```

---

## 📖 Examples

### Complete Workflow

```bash
# 1. Login to your account
xtr login
# Enter username and token when prompted

# 2. Initialize repository
xtr init

# 3. Stage files
xtr add .

# 4. Create new code
xtr push --name "My Project" --readme "Description" --tags "js,node"

# 5. Update code
xtr add src/new-file.js
xtr push --message "Added new feature"
```

### Working with Remotes

```bash
# Add remote
xtr remote add origin https://extra.codes/d/EC1234567890-abc123

# Pull from remote (if only one remote, no argument needed)
xtr pull

# Pull from specific remote
xtr pull origin

# Push to remote
xtr add .
xtr push
```

### Cloning and Pulling

```bash
# Clone a repository
xtr clone https://extra.codes/d/EC1234567890-abc123

# Pull updates (uses configured remote)
xtr pull

# Pull from specific URL
xtr pull https://extra.codes/d/EC1234567890-abc123
```

### Library Usage Example

```javascript
const xtr = require('xtr-cli');

async function deployProject() {
  // Login (if not already logged in)
  await xtr.loginCommand();
  
  // Initialize
  await xtr.initCommand();
  
  // Stage and push
  await xtr.addCommand('.');
  await xtr.pushCommand({
    name: 'My Project',
    readme: 'Auto-deployed project',
    tags: 'automated,ci'
  });
  
  console.log('Deployment complete!');
}

deployProject();
```

---

## 🔧 Requirements

- **Node.js** >= 14.0.0
- **npm** or **yarn**

---

## 🐛 Troubleshooting

### "Error: Could not find dist/index.js"

Run `npm run build` in the xtr-cli directory, or reinstall the package.

### "Error: Invalid URL format"

Make sure the URL follows the format: `https://extra.codes/d/EC1234567890-abc123`

### "Error: Not authenticated" or "Missing or invalid authorization header"

Run `xtr login` to authenticate with your account:
```bash
xtr login
```

Make sure you have generated an account token from `/profile/[username]/settings` (Account Token tab) on [extra.codes](https://extra.codes).

### "Error: No remote configured"

When running `xtr pull` without arguments, you need at least one remote configured. Either:
- Provide a URL: `xtr pull <url>`
- Add a remote: `xtr remote add <name> <url>`

### "Received HTML instead of JSON"

This usually means the API endpoint is incorrect. The API URLs are configured by the package developer. For local development, you can override them using environment variables:
- `XTR_API_URL` - Override API URL
- `XTR_FILES_API_URL` - Override Files API URL

---

## 📝 License

MIT

---

## 🔗 Links

- **ExtraCodes Platform**: [extra.codes](https://extra.codes)
- **Discord Community**: [Join our server](https://discord.gg/zU4DtNUCnf)
- **npm Package**: [xtr-cli](https://www.npmjs.com/package/xtr-cli)

---

<div align="center">

**Made with ❤️ by [ExtraCodes Team](https://extra.codes)**

</div>
