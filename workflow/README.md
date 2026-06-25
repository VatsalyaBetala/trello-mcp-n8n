# Workflow

Download the n8n workflow JSON and place it here as `trello-mcp-v2.json`.

## How to export from n8n

1. Open the Trello MCP - v2 workflow in n8n
2. Click the **⋯** menu (top right) → **Download**
3. Save the file as `trello-mcp-v2.json` in this directory

## Before committing

The exported JSON will contain your credential IDs. Before pushing to GitHub:

1. Open the JSON file
2. Find any `credentials` blocks — they look like:
   ```json
   "credentials": {
     "trelloApi": {
       "id": "abc123",
       "name": "Trello"
     }
   }
   ```
3. Replace the `id` value with `"YOUR_CREDENTIAL_ID"`:
   ```json
   "credentials": {
     "trelloApi": {
       "id": "YOUR_CREDENTIAL_ID",
       "name": "Trello"
     }
   }
   ```

This is safe — users replace this with their own credential ID after import anyway.
