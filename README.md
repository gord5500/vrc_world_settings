# VRChat World Settings

A GitHub Pages application that hosts JSON configuration files for VRChat world settings. Players can fetch this information on world load.

## Overview

This repository contains JSON files with settings for different VRChat worlds. The files are automatically deployed to GitHub Pages and can be accessed via HTTP requests from within VRChat worlds.

## Available Files

- **`world_settings.json`** - Combined settings for multiple worlds
- **`wrld_example_001.json`** - Settings for Example World 1
- **`wrld_example_002.json`** - Settings for Example World 2

## Usage

### Fetching Settings in VRChat

Players can fetch world settings on load using the following URL pattern:

```
https://gord5500.github.io/vrc_world_settings/world_settings.json
https://gord5500.github.io/vrc_world_settings/wrld_example_001.json
```

### Example Implementation

In your VRChat world, you can use VRChat's networking capabilities to fetch these settings:

```csharp
// Example using VRCUrl
VRCUrl settingsUrl = new VRCUrl("https://gord5500.github.io/vrc_world_settings/wrld_example_001.json");
VRCStringDownloader.LoadUrl(settingsUrl, (IVRCStringDownload result) => {
    if (result.Error == null) {
        string jsonData = result.Result;
        // Parse and apply settings
    }
});
```

## Deployment

The JSON files are automatically deployed to GitHub Pages through GitHub Actions.

### Automatic Deployment

The workflow is triggered automatically on:
- Push to `main` branch (when JSON files or workflow file changes)
- Manual trigger from the Actions tab
- Repository dispatch events

### Manual Deployment

You can manually trigger a deployment from the GitHub Actions tab:
1. Go to the "Actions" tab in this repository
2. Select "Deploy JSON to GitHub Pages" workflow
3. Click "Run workflow"

### Repository Dispatch Event

You can trigger deployment programmatically using the repository dispatch event:

```bash
curl -X POST \
  -H "Accept: application/vnd.github.v3+json" \
  -H "Authorization: token YOUR_TOKEN" \
  https://api.github.com/repos/gord5500/vrc_world_settings/dispatches \
  -d '{"event_type":"deploy-settings"}'
```

## JSON Structure

### World Settings File

```json
{
  "worldId": "wrld_example_001",
  "worldName": "Example World 1",
  "description": "A demonstration world with standard settings",
  "capacity": 20,
  "settings": {
    "movement": {
      "walkSpeed": 2.0,
      "runSpeed": 4.0,
      "jumpImpulse": 3.0,
      "gravity": -9.81
    },
    "spawn": {
      "spawnPoint": { "x": 0.0, "y": 1.0, "z": 0.0 },
      "respawnHeight": -10.0
    },
    "voice": {
      "voiceDistanceFar": 25.0,
      "voiceGain": 15.0
    }
  }
}
```

## Adding New Worlds

To add a new world:

1. Create a new JSON file following the structure above (e.g., `wrld_your_world_id.json`)
2. Commit and push to the `main` branch
3. The workflow will automatically deploy the changes

## Development

### Validating JSON

All JSON files are automatically validated during the deployment process. You can also validate locally:

```bash
jq empty your_file.json
```

### Testing Locally

You can test the JSON files locally by serving them with a simple HTTP server:

```bash
python -m http.server 8000
# Then access http://localhost:8000/world_settings.json
```

## Branch Protection

This repository uses a CODEOWNERS file to control who can create and merge branches. Only approved code owners can approve and merge pull requests.

## License

This project is open source and available under the MIT License.