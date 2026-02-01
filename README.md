# Lenovo Legion Toolkit Plugins

Official plugin repository for Lenovo Legion Toolkit.

## For Users

Download plugins from the [Releases](https://github.com/Crs10259/LenovoLegionToolkit-Plugins/releases) page or install via the Plugin Manager in Lenovo Legion Toolkit.

## For Developers

### Prerequisites

- .NET 8.0 SDK
- Windows: PowerShell 5.1+ or Inno Setup 6 (for installer creation)
- Linux/macOS: PowerShell Core or zip command

### Building Plugins

#### Windows (PowerShell)

```powershell
# Build all plugins (Release)
make.bat

# Build all plugins (Debug)
make.bat debug

# Build a specific plugin
make.bat ViveTool

# Build a specific plugin (Debug)
make.bat ViveTool d

# Create ZIP packages for all plugins
make.bat zip

# Clean all build outputs
make.bat clean

# Show help
make.bat help
```

#### Linux/macOS (Bash)

```bash
# Build all plugins (Release)
./build.sh

# Build all plugins (Debug)
./build.sh debug

# Build a specific plugin
./build.sh ViveTool

# Build a specific plugin (Debug)
./build.sh ViveTool d

# Create ZIP packages for all plugins
./build.sh zip

# Clean all build outputs
./build.sh clean

# Show help
./build.sh help
```

#### Using dotnet CLI Directly

```bash
# Build all plugins
dotnet build

# Build a specific plugin
dotnet build ./plugins/ViveTool/LenovoLegionToolkit.Plugins.ViveTool.csproj

# Build with Release configuration
dotnet build ./plugins/ViveTool/LenovoLegionToolkit.Plugins.ViveTool.csproj -c Release

# Publish a plugin
dotnet publish ./plugins/ViveTool/LenovoLegionToolkit.Plugins.ViveTool.csproj -c Release -o ./publish/ViveTool
```

### Creating Release Packages

1. Build all plugins in Release mode:
   ```bash
   make.bat zip
   ```

2. This creates:
   - `ViveTool.zip`
   - `CustomMouse.zip`
   - `NetworkAcceleration.zip`
   - `ShellIntegration.zip`

3. Each ZIP contains the plugin DLL and all dependencies, ready for distribution via the Plugin Manager.

### Plugin Structure

```
LenovoLegionToolkit-Plugins/
├── plugins/                       # Plugin source code
│   ├── SDK/                       # Plugin SDK (development dependency)
│   ├── ViveTool/
│   ├── CustomMouse/
│   ├── NetworkAcceleration/
│   └── ShellIntegration/
├── build/                         # Build output (created after build)
│   └── plugins/
├── .github/
│   └── workflows/
│       └── build.yml              # GitHub Actions CI/CD
├── make.bat                       # Windows build script
├── build.sh                       # Linux/macOS build script
├── store.json                     # Plugin store metadata
└── README.md
```

### Creating a New Plugin

1. Create a new class library project:
   ```bash
   dotnet new classlib -n LenovoLegionToolkit.Plugins.MyPlugin -o ./plugins/MyPlugin
   ```

2. Add reference to the SDK:
   ```bash
   dotnet add ./plugins/MyPlugin/LenovoLegionToolkit.Plugins.MyPlugin.csproj reference ./plugins/SDK/LenovoLegionToolkit.Plugins.SDK.csproj
   ```

3. Implement the `IPlugin` interface from `LenovoLegionToolkit.Lib.Plugins`

4. Update `store.json` to include your plugin:
   ```json
   {
     "icon": "Apps24",
     "iconBackground": "#0078D4",
     "fileSize": 0,
     "name": "MyPlugin",
     "downloadUrl": "https://github.com/Crs10259/LenovoLegionToolkit-Plugins/releases/download/MyPlugin-v1.0.0/MyPlugin.zip",
     "isSystemPlugin": false,
     "description": "My custom plugin description",
     "version": "1.0.0",
     "changelog": "Version 1.0.0 (2026-02-01)\n- Initial release",
     "releaseDate": "2026-02-01T00:00:00Z",
     "fileHash": "",
     "author": "YourName",
     "dependencies": null,
     "id": "myplugin",
     "tags": ["utility"],
     "minimumHostVersion": "2.0.0"
   }
   ```

5. Build and create ZIP package:
   ```bash
   make.bat MyPlugin
   make.bat zip
   ```

6. Create a GitHub Release with the ZIP file as an asset.

### GitHub Actions CI/CD

The repository includes a GitHub Actions workflow (`.github/workflows/build.yml`) that:

- Automatically builds all plugins on push to `master`
- Supports manual workflow dispatch for specific plugin builds
- Can create releases when triggered with a version number

#### Workflow Dispatch

1. Go to [Actions > Build Plugins](https://github.com/Crs10259/LenovoLegionToolkit-Plugins/actions/workflows/build.yml)
2. Click "Run workflow"
3. Select:
   - **Plugin**: Specific plugin to build (leave empty for all)
   - **Version**: Version number for release (e.g., `1.0.0`)

When a version is provided, the workflow will:
- Build all plugins
- Create ZIP packages
- Create a GitHub Release with the ZIP assets
- Update `store.json` with new checksums and versions

### Store URL

Lenovo Legion Toolkit fetches plugins from:
```
https://raw.githubusercontent.com/Crs10259/LenovoLegionToolkit-Plugins/master/store.json
```

### Contributing

1. Fork this repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Make your changes
4. Build and test: `make.bat`
5. Submit a pull request

## License

MIT License
