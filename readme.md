# N8N Chat Interface Node

A powerful N8N custom node that serves a fully customizable React chat interface as static content. This node encapsulates a complete AI chat application and allows you to deploy it with custom theming, feedback collection, and webhook integration without requiring additional servers or port management.

## 🌟 Features

- 🎨 **Complete Theme Customization**: Configure all colors for both light and dark modes with HSL values
- 🔧 **Dynamic Configuration**: Set chat API URL, title, and all UI settings through node parameters
- 📦 **Self-Contained**: No external servers, databases, or port binding required
- 🚀 **Production Ready**: Builds and serves optimized React app as static content
- 🔍 **SPA Routing**: Handles single-page application routing correctly
- 👍 **Feedback System**: Built-in thumbs up/down feedback collection with configurable endpoints
- 🔗 **Webhook Integration**: Convert static asset paths to webhook calls for dynamic serving
- 📱 **Responsive Design**: Mobile-first design that works on all screen sizes
- 🌐 **Markdown Support**: Rich text rendering in chat messages with reference support

## 📦 Installation

### 🌟 NPM Installation (Recommended)

Install as a community node using N8N's built-in community node manager:

1. **Via N8N UI** (Easiest):
   - Go to **Settings** → **Community Nodes**
   - Click **Install a community node**
   - Enter: `n8n-nodes-agent-chat-interface`
   - Click **Install**

2. **Via NPM Command**:
   ```bash
   npm install n8n-nodes-agent-chat-interface
   ```

3. **Restart N8N**: The node will be automatically available after restart

### 🛠️ Development Installation

For development or custom modifications:

1. **Clone Repository**: Get the source code from the repository
2. **Install Dependencies** (from project root):
   ```bash
   # Install all workspace dependencies
   yarn install
   
   # Build chat interface library first (required dependency)
   yarn workspace chat-interface-lib build
   
   # Build the n8n node (includes React app bundling)
   yarn workspace n8n-nodes-agent-chat-interface build
   ```
3. **Link Locally**: 
   ```bash
   # From the n8n-nodes-agent-chat-interface directory
   npm link
   
   # In your N8N installation directory
   npm link n8n-nodes-agent-chat-interface
   ```
4. **Restart N8N**: Restart your N8N instance to load the new node

### ✅ Verify Installation

After installation, look for "Chat Interface" in the N8N node palette under the **"Output"** category.

### 📋 Requirements

- **N8N Version**: Compatible with N8N 0.200.0+
- **Node.js**: Version 16.9+ required
- **NPM**: Latest version recommended

> **Note**: This package includes pre-built React assets and requires no additional dependencies beyond what's included in the npm package.

## 🚀 Quick Start Guide

### Basic Chat Interface Setup

1. **Add the Node**: Drag "Chat Interface" node to your workflow
2. **Configure Mandatory Parameters**:
   - **Request Path**: `/` (serves the main chat interface)
   - **Chat API URL**: `https://your-api.com/chat` (your backend endpoint)
   - **Chat Title**: `My AI Assistant` (displayed in header)

3. **Test the Setup**:
   ```json
   Input: { "requestPath": "/" }
   Output: { 
     "statusCode": 200, 
     "content": "PGh0bWw+Li4uPC9odG1sPg==", 
     "contentType": "text/html" 
   }
   ```

### Advanced Setup with Feedback

```json
{
  "requestPath": "/",
  "chatApiUrl": "https://api.example.com/chat",
  "chatTitle": "Customer Support Bot",
  "webhookUrl": "https://your-n8n-instance.com/webhook/your-webhook-id",
  "agentSid": "user123@company.com",
  "feedbackEnabled": true,
  "thumbsUpUrl": "https://api.example.com/feedback/positive",
  "thumbsDownUrl": "https://api.example.com/feedback/negative",
  "lightThemeColors": {
    "primary": "220 98% 61%",
    "secondary": "220 20% 95%"
  },
  "darkThemeColors": {
    "primary": "220 98% 61%",
    "secondary": "220 20% 15%"
  }
}
```

> **Note**: The `webhookUrl` should be the webhook URL of the same N8N flow where you placed this Chat Interface node. This allows the chat interface to serve all its assets (CSS, JS, images) through N8N webhooks instead of requiring a separate web server.

## ⚙️ Node Parameters

### 🔴 Mandatory Parameters

These parameters are **required** for the chat interface to function:

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| **Request Path** | `string` | ✅ | `/` | The path to serve (/, /assets/style.css, etc.) |
| **Chat API URL** | `string` | ✅ | - | **MUST be configured** - Your chat backend API endpoint |
| **Chat Title** | `string` | ✅ | `AI Assistant Chat` | Title displayed in the chat interface header |

### 🔧 Core Configuration

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| **Webhook URL** | `string` | `""` | **The webhook URL of the N8N flow containing this node**. Enables serving assets via N8N webhooks by converting all paths to `?path=` queries |
| **Agent SID** | `string` | `""` | Optional user identifier (email, username, or ID) to include in chat API requests for user tracking and personalization |

### 👍 Feedback Configuration

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| **Enable Feedback** | `boolean` | `false` | Enable thumbs up/down feedback buttons on AI responses |
| **Thumbs Up URL** | `string` | `""` | POST endpoint for positive feedback (required if feedback enabled) |
| **Thumbs Down URL** | `string` | `""` | POST endpoint for negative feedback (required if feedback enabled) |

### 🎨 Theme Configuration

#### Light Theme Colors
All colors use HSL format **without** the `hsl()` wrapper (e.g., `220 98% 61%`):

| Parameter | Default | Description |
|-----------|---------|-------------|
| **Background** | `0 0% 100%` | Main page background |
| **Foreground** | `0 0% 3.9%` | Primary text color |
| **Card** | `0 0% 100%` | Chat components background |
| **Card Foreground** | `0 0% 3.9%` | Text on cards |
| **Primary** | `220 98% 61%` | Button colors, send button, links |
| **Primary Foreground** | `0 0% 98%` | Text on primary colored elements |
| **Secondary** | `0 0% 96.1%` | Secondary UI elements |
| **Secondary Foreground** | `0 0% 9%` | Text on secondary elements |
| **Muted** | `0 0% 96.1%` | Subtle backgrounds |
| **Muted Foreground** | `0 0% 45.1%` | Muted text, timestamps |
| **Border** | `0 0% 89.8%` | Component borders |
| **Input** | `0 0% 89.8%` | Input field backgrounds |

#### Dark Theme Colors
Same structure as light theme with dark-optimized defaults:

| Parameter | Default | Description |
|-----------|---------|-------------|
| **Background** | `0 0% 3.9%` | Dark background |
| **Foreground** | `0 0% 98%` | Light text |
| **Primary** | `220 98% 61%` | Accent color (often same as light) |
| **Secondary** | `0 0% 14.9%` | Dark secondary elements |
| **Border** | `0 0% 14.9%` | Dark borders |

## 📡 API Integration

### Chat Message Payload

When users send messages, the chat interface makes a POST request to your **Chat API URL** with this payload:

```json
{
  "message": "User's message text",
  "guid": "session-uuid-12345",
  "agent_sid": "optional-user-identifier"
}
```

#### Payload Fields:
- **message** (`string`): The user's input text
- **guid** (`string`): Unique session identifier (generated per chat session)
- **agent_sid** (`string`, optional): User identifier from the Agent SID parameter, SharePoint context, or other sources

### Expected Chat API Response

Your backend should respond with this JSON structure:

```json
{
  "output": "AI response text with **markdown** support",
  "references": [
    {
      "name": "Document Title",
      "content": "Optional document content for sidebar",
      "url": "https://example.com/doc" 
    }
  ]
}
```

#### Response Fields:
- **output** (`string`, required): The AI's response text (supports markdown)
- **references** (`array`, optional): List of reference documents/links
  - **name** (`string`): Display name for the reference
  - **content** (`string`, optional): Full document content (opens in sidebar)
  - **url** (`string`, optional): External URL (opens in new tab)

### Feedback Payload

When feedback is enabled and users click thumbs up/down, a POST request is sent to the configured feedback URLs:

```json
{
  "messageId": "unique-message-id",
  "messageContent": "The AI response that was rated",
  "feedback": "positive",
  "timestamp": "2024-01-01T12:00:00.000Z"
}
```

#### Feedback Fields:
- **messageId** (`string`): Unique identifier for the message
- **messageContent** (`string`): The full AI response text
- **feedback** (`string`): Either `"positive"` or `"negative"`
- **timestamp** (`string`): ISO timestamp of when feedback was given

## 🛣️ URL Routing & Asset Serving

### Supported Request Paths

| Path Pattern | Behavior | Content Type |
|--------------|----------|--------------|
| `/` | Serves main chat interface | `text/html` |
| `/assets/*.js` | Serves JavaScript bundles | `application/javascript` |
| `/assets/*.css` | Serves stylesheets | `text/css` |
| `/assets/*.png`, `.jpg`, etc. | Serves images | `image/*` |
| `/any-route` (no extension) | Serves index.html (SPA routing) | `text/html` |
| Invalid paths | Returns 404 | `text/plain` |

### Response Format

All responses follow this structure:

```json
{
  "statusCode": 200,
  "content": "base64-encoded-content",
  "contentType": "text/html"
}
```

## 🎨 Theme Examples

### Corporate Blue Theme
```json
{
  "lightThemeColors": {
    "primary": "220 98% 61%",
    "secondary": "220 20% 95%",
    "background": "220 20% 98%"
  },
  "darkThemeColors": {
    "primary": "220 98% 61%",
    "secondary": "220 20% 15%",
    "background": "220 20% 8%"
  }
}
```

### Success Green Theme
```json
{
  "lightThemeColors": {
    "primary": "142 76% 36%",
    "secondary": "142 20% 95%"
  },
  "darkThemeColors": {
    "primary": "142 76% 46%",
    "secondary": "142 20% 15%"
  }
}
```

### Creative Purple Theme
```json
{
  "lightThemeColors": {
    "primary": "262 83% 58%",
    "secondary": "262 20% 95%"
  },
  "darkThemeColors": {
    "primary": "262 83% 68%",
    "secondary": "262 20% 15%"
  }
}
```

## 🔧 Development & Building

### Development Workflow

```bash
# Start development mode (watches for changes)
yarn workspace n8n-nodes-agent-chat-interface dev

# Clean build artifacts
yarn workspace n8n-nodes-agent-chat-interface clean

# Full production build
yarn workspace n8n-nodes-agent-chat-interface build
```

### Publishing to NPM

To publish as a community node:

```bash
# Build the package
yarn workspace n8n-nodes-agent-chat-interface build

# Navigate to package directory
cd packages/n8n-nodes-agent-chat-interface

# Publish to npm
npm publish
```

> **Note**: Ensure you have proper npm permissions and the package version is updated before publishing.

### Build Process

1. **Dependency Check**: Ensures chat-interface-lib is built
2. **React App Build**: Creates optimized production bundle
3. **Asset Bundling**: Packages all static assets
4. **Node Compilation**: Builds TypeScript node code
5. **Cleanup**: Removes temporary build directories after 1 hour

## 📁 Project Structure

```
packages/n8n-nodes-agent-chat-interface/
├── src/
│   ├── ChatInterfaceNode.node.ts    # Main N8N node implementation
│   ├── ChatInterfaceBuilder.ts      # React app build system
│   └── RuntimeThemeInjector.ts      # Runtime configuration injection
├── dist/                            # Compiled N8N node
├── temp-builds/                     # Temporary React builds (auto-cleanup)
├── assets/                          # Pre-built React app assets
├── package.json
├── tsconfig.json
├── chatInterface.svg                # Node icon
└── README.md
```

## ⚡ Performance Notes

- **Build Time**: ~30-60 seconds for initial React app build
- **Runtime Serving**: ~3ms response time with optimized caching
- **Memory Usage**: <50MB footprint with asset serving
- **Asset Bundling**: All dependencies bundled at build time (no runtime compilation)
- **Auto Cleanup**: Temporary builds removed after 1 hour to prevent disk usage buildup

## 🔍 Troubleshooting

### Common Issues

**Node not appearing in N8N palette:**
- Verify the npm package was installed successfully: `npm list n8n-nodes-agent-chat-interface`
- Check N8N logs for loading errors
- Ensure N8N instance was restarted after installation
- For community nodes, verify the package name: `n8n-nodes-agent-chat-interface`

**Chat interface not loading:**
- Verify Chat API URL is accessible
- Check browser developer console for errors
- Ensure request path is correct (`/` for main interface)

**Feedback not working:**
- Verify `feedbackEnabled` is set to `true`
- Check that feedback URLs are properly configured
- Monitor network requests in browser dev tools

**Theme not applying:**
- Ensure HSL values are in correct format (no `hsl()` wrapper)
- Check that theme colors are properly configured
- Verify no syntax errors in color values

### Development Debugging

```bash
# Enable verbose logging
NODE_ENV=development yarn build

# Test specific paths
curl "http://your-n8n-instance/webhook/chat-interface?path=/"
curl "http://your-n8n-instance/webhook/chat-interface?path=/assets/style.css"
```

### Example flow

```json
{
  "name": "Chat Interface Flow",
  "nodes": [
    {
      "parameters": {
        "path": "chat-interface",
        "responseMode": "responseNode",
        "options": {}
      },
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 2,
      "position": [
        48,
        160
      ],
      "id": "5ad9e94c-996a-4135-a947-25f5a5da12c8",
      "name": "Webhook",
      "webhookId": "chat-interface"
    },
    {
      "parameters": {
        "requestPath": "={{ $json.query?.path ?? \"/\" }}",
        "chatApiUrl": "",
        "chatTitle": "Chat Interface",
        "webhookUrl": "={{ $json.webhookUrl }}",
        "lightThemeColors": {},
        "darkThemeColors": {}
      },
      "type": "n8n-nodes-agent-chat-interface.chatInterface",
      "typeVersion": 1,
      "position": [
        272,
        160
      ],
      "id": "6cc4b7b0-44d7-4e5a-ac71-0ef9234c69e0",
      "name": "Chat Interface"
    },
    {
      "parameters": {
        "jsCode": "return [\n  {\n    json: {\n      statusCode: $input.first().json.statusCode\n    },\n    binary: {\n      data: {\n        data: $input.first().json.content,\n        mimeType: $input.first().json.contentType,\n        fileName: 'aaa'\n      }\n    }\n  }\n];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        496,
        160
      ],
      "id": "11706cfb-197d-4221-ac6a-8c6b8fcea8e4",
      "name": "Code"
    },
    {
      "parameters": {
        "respondWith": "binary",
        "options": {
          "responseCode": "={{ $json.statusCode }}"
        }
      },
      "type": "n8n-nodes-base.respondToWebhook",
      "typeVersion": 1.4,
      "position": [
        720,
        160
      ],
      "id": "cd399df6-f325-495c-af62-a5644e41b422",
      "name": "Respond to Webhook"
    }
  ],
  "pinData": {},
  "connections": {
    "Webhook": {
      "main": [
        [
          {
            "node": "Chat Interface",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Chat Interface": {
      "main": [
        [
          {
            "node": "Code",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Code": {
      "main": [
        [
          {
            "node": "Respond to Webhook",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": true,
  "settings": {
    "executionOrder": "v1"
  },
  "versionId": "93438130-63cd-4799-ac79-3df509bb8dda",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "123123123"
  },
  "id": "123123123",
  "tags": []
}
```

## 📄 License

The source code is not available, but feel free to use this node

---

## 🚀 Ready to Deploy?

1. **Configure your Chat API URL** (mandatory)
2. **Set up your feedback endpoints** (optional)
3. **Customize your theme colors** (optional)  
4. **Test with Request Path** = `/`
5. **Deploy to your N8N workflows**

Your chat interface will be served as static content with full customization and zero infrastructure overhead! 

## Donation

Did you like my node? Pay me a coffee :)

[![Donate with PayPal](https://img.shields.io/badge/🍵%20Donate-PayPal-blue)](https://www.paypal.com/donate/?business=9XAQZP7QF2RUG&no_recurring=0&item_name=You+can+ask+for+features+using+the+email%3A+lelemm2+at+gmail+dot+com&currency_code=USD)
