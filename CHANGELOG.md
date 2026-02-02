# Changelog

## v1.1.0

### Major Changes
- **Local Player API for commands**: Playback controls (play, pause, skip, seek, volume) now go directly to Plexamp's local HTTP API instead of routing through the Plex server. This eliminates the most common cause of "buttons not working" reported by users. Commands are faster, more reliable, and don't require the server to relay them.
- **Timeline poll for real-time playback position**: Replaced the old `/status/sessions` server poll with Plexamp's `/player/timeline/poll` endpoint. The progress bar and time display now show actual playback position from the player instead of interpolated guesswork. Polling interval reduced from 2s to 1s for more responsive feedback.
- **Server fallback**: If the local player is unreachable, commands and polling automatically fall back to the original server relay method so nothing breaks.

### New Features
- **Shuffle button**: New action that toggles shuffle on/off. Button icon lights up with the accent color when shuffle is active.
- **Repeat button**: New action that cycles through repeat modes: Off → All → One. Icon shows the current mode with accent color and state label.
- **Dial press action selector**: The dial press on the Now Playing Strip can now be configured to Play/Pause, Toggle Shuffle, or Cycle Repeat via a dropdown in strip settings.
- **Touch strip visual feedback**: Tapping the touch strip or using dial controls now shows a brief overlay on the active strip panel for 1.5 seconds. Displays the action taken (PLAYING/PAUSED with play/pause icon, NEXT/PREVIOUS, VOLUME with fill bar, SHUFFLE ON/OFF, REPEAT OFF/ALL/ONE).
- **Touch strip play/pause**: Tapping the touch strip toggles play/pause.
- **Scrolling text**: Long artist names, album titles, and track names that don't fit on a strip panel now scroll automatically. Text pauses briefly, scrolls left, then resets. Each panel scrolls independently.
- **Player URL setting**: New "Plexamp Player" section in settings with a configurable Player URL. Defaults to `http://localhost:32500` for headless Plexamp. Desktop users can enter their Plexamp's port.
- **Debug logging**: New "Enable debug logging" checkbox in Advanced settings. When on, logs detailed API requests, responses, and connection state to the browser console. Tokens are automatically sanitized in log output.
- **Dual test buttons**: Separate "Test Player" and "Test Server" buttons in settings. Test Player checks the local Plexamp connection. Test Server checks the Plex server for metadata access.

### Changes
- Time sync offset now defaults to 0 (was 1500). With real-time position from the timeline poll, the offset is no longer needed in normal operation. Only applies when using server fallback mode.
- Shuffle and repeat state sync from the player's timeline on every poll.
- Volume state syncs from the player's reported volume on each poll.
- Settings UI reorganized: Player settings and Server settings are now separate sections for clarity.
- Plex tokens are masked in debug log output for safe sharing.

### Technical Notes
- Commands include `X-Plex-Client-Identifier` header and incrementing `commandID` per the Plex Remote Control API spec.
- Timeline poll uses `includeMetadata=1` parameter. If the player returns track metadata in the response, it's used directly. If not, metadata is fetched from the server as before.
- Album art is still fetched from the Plex server (requires token). The timeline provides server connection info (address, port, protocol, token) which is used to construct art URLs.
- Shuffle uses `/player/playback/setParameters?shuffle=0` or `shuffle=1`.
- Repeat uses `/player/playback/setParameters?repeat=0` (off), `repeat=1` (all), or `repeat=2` (one track).

## v1.0.1

### New Features
- **Dial controls**: Configurable dial actions for the Now Playing Strip. Choose between Next/Previous on rotate, Volume on rotate, or None. All modes support Play/Pause on dial press.
- **Configurable text color**: Choose from White, Light Gray, Orange, Amber, or Black for text on both buttons and the strip. Useful for matching lighter Stream Deck themes.
- **Dynamic color toggle**: Option to disable dynamic accent colors extracted from album art. When off, accents stay on the default orange.
- **macOS support**: Added install.sh for macOS and macOS platform entry in the manifest.

### Changes
- Appearance settings now appear on both button and strip property inspectors
- Layout refreshes immediately when appearance settings change
- Manifest TriggerDescription updated to reflect dial functionality

## v1.0.0

Initial release.

- Album art display with dominant color extraction
- Now Playing strip with configurable panels spanning all 4 dials
- Play/Pause, Previous, Next button actions
- Track Info button (codec, bitrate, track number)
- Time Elapsed button with progress bar
- Hold-to-seek on Previous/Next buttons
- Configurable sync offset for Plex reporting delay
- Interpolated progress (200ms render, 2s poll)
- Test Connection button in settings
