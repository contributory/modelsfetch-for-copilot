# Copilot Custom Endpoint Model Fetcher

VS Code Extension that automatically fetches model lists from your Custom Endpoint and configures them directly into GitHub Copilot on VS Code (via the `chatLanguageModels.json` file).

---

## 🌟 Key Features

- **Automatic Configuration Discovery**: Precisely locates the VS Code `chatLanguageModels.json` configuration file across all platforms (Windows, macOS, Linux, VS Code Insiders).
- **Full OpenAI Standard Compatibility**: Sends requests to `<Base URL>/models` using an API Key to retrieve all available models.
- **Smart Parameter Resolution**:
  - Detects models with **Vision** support (e.g., `gpt-4o`, `claude-3-5`, `gemini`).
  - Detects and enables **Thinking/Reasoning** for reasoning models (e.g., `deepseek-reasoner`, `deepseek-r1`).
  - Automatically populates optimal **maxInputTokens** & **maxOutputTokens** for popular model types (e.g., DeepSeek, Claude, GPT, Gemini).
  - Automatically enables **toolCalling: true** to ensure models appear in the Copilot Chat model picker.
- **Interactive UI**:
  - Select existing configurations or create new ones directly from the Command Palette.
  - Quick multi-select multiple models at once using QuickPick lists.
  - Preserves your existing custom configurations (token limits, custom names) without overwriting data.
- **Security First**: Maintains VS Code's Secret Storage mechanism (`${input:chat.lm.secret.xxx}`) and only uses the API Key temporarily for fetching, never storing keys in plaintext unless explicitly requested.

---

## 🚀 How to Use

1. Press `Ctrl + Shift + P` (or `Cmd + Shift + P` on macOS) to open the **Command Palette**.
2. Search for and select the command: **`Copilot Custom Endpoint: Fetch and Configure Models`**.
3. **Select Provider**:
   - Choose an existing Custom Endpoint provider from your `chatLanguageModels.json` file.
   - Or select **`Create New Custom Endpoint Provider...`** to start configuring a new provider.
4. **Enter Connection Details**:
   - Enter **Base URL** (e.g., `https://api.deepseek.com/v1` or `http://localhost:11434/v1`).
   - Enter **API Key** (This key is only used to fetch the model list and is not stored in plaintext if you're using VS Code's security mechanism).
   - Select **API Type** (`chat-completions`, `messages`, or `responses`).
5. **Select Models**:
   - Check the models you want to add to VS Code Copilot Chat from the returned list.
6. **Complete**:
   - The system will automatically merge the new configuration into `chatLanguageModels.json`.
   - A confirmation notification will appear, and you can select **Open Config File** to view and verify directly.

---

## 🛠️ Development & Testing

If you want to contribute or package the extension yourself:

```bash
# Install dependencies
npm install

# Run automated tests (Unit Tests)
npm run test

# Compile TypeScript source
npm run compile

# Package as .vsix installable file
npx --yes @vscode/vsce package --allow-missing-repository --skip-license
```

---

## 📦 Reference Configuration (`chatLanguageModels.json` after setup)

After running the extension, your `chatLanguageModels.json` configuration will look like this:

```json
[
  {
    "name": "DeepSeek",
    "vendor": "customendpoint",
    "apiKey": "${input:chat.lm.secret.deepseek-key}",
    "apiType": "chat-completions",
    "models": [
      {
        "id": "deepseek-chat",
        "name": "DeepSeek Chat",
        "url": "https://api.deepseek.com/v1/chat/completions",
        "toolCalling": true,
        "vision": false,
        "maxInputTokens": 64000,
        "maxOutputTokens": 4096,
        "apiType": "chat-completions"
      },
      {
        "id": "deepseek-reasoner",
        "name": "DeepSeek R1 (Reasoner)",
        "url": "https://api.deepseek.com/v1/chat/completions",
        "toolCalling": true,
        "vision": false,
        "maxInputTokens": 64000,
        "maxOutputTokens": 8192,
        "thinking": true,
        "apiType": "chat-completions"
      }
    ]
  }
]
```

---

## 📄 License

This project is licensed under the **GNU General Public License v3.0 (GPL-3.0)**.

See the [LICENSE](LICENSE) file for the full license text.

In summary, GPL-3.0 grants you the freedom to:
- Use the software for any purpose
- Study and modify the source code
- Distribute copies of the software
- Distribute your modified versions

With the condition that any distributed copies or derivative works must also be licensed under GPL-3.0 and include the source code.
```