# AionUi Build Guide

## Prerequisites

- Node.js >= 22.14.0
- Bun >= 1.2.8
- Git

## Build Commands

### Development

```bash
# Install dependencies
bun install

# Start development server
bun run dev
```

### Build Windows

```bash
bun run build-win
```

Output: `out/AionUi-{version}-win-x64.exe` and `out/AionUi-{version}-win-x64.zip`

### Build macOS

```bash
bun run build-mac
```

Output: `out/AionUi-{version}-mac-arm64.dmg` and `out/AionUi-{version}-mac-arm64.zip`

### Build macOS Universal (Intel + Apple Silicon)

```bash
bun run build-mac-universal
```

Output: `out/AionUi-{version}-mac-universal.dmg`

### Build Linux

```bash
bun run build-linux
```

Output: `out/AionUi-{version}-linux-arm64.zip` and `out/AionUi-{version}-linux-arm64.AppImage`

### Build All Platforms

```bash
bun run build-all
```

## Other Commands

| Command                 | Description                  |
| ----------------------- | ---------------------------- |
| `bun run typecheck`     | Run TypeScript type checking |
| `bun run lint`          | Check code style             |
| `bun run lint:fix`      | Auto-fix lint issues         |
| `bun run format`        | Format code with oxfmt       |
| `bun run test`          | Run tests                    |
| `bun run test:coverage` | Run tests with coverage      |

## Chinese Mirror Configuration

The project uses npmmirror for Chinese network environments:

- **Electron**: `https://npmmirror.com/mirrors/electron/`
- **Bun**: Use GitHub releases directly

## Notes

- For offline builds, manually download Electron binary to `~/.cache/electron/` (Linux) or `%LOCALAPPDATA%\electron\Cache\` (Windows)
- Set `SKIP_BUNDLED_BUN=1` to skip bun runtime bundling if not needed
