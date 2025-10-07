# PWA Testing Guide

## Overview
The Vegan Campsite Cookbook is now a Progressive Web App (PWA) that can be installed on mobile devices and desktop computers.

## What Was Added

### Meta Tags
- `mobile-web-app-capable` - Enables app mode on Android
- `apple-mobile-web-app-capable` - Enables app mode on iOS  
- `apple-mobile-web-app-status-bar-style` - Controls iOS status bar
- SVG icon embedded as data URL (green tree design)

### JavaScript Features
- Dynamic manifest generation with app metadata
- SVG to PNG icon conversion for broader compatibility
- iOS startup image generation
- Install prompt event listener

## Testing Instructions

### Chrome/Edge (Desktop & Android)

1. **Open the site** in Chrome or Edge
2. **Look for install prompt**:
   - Desktop: Look for install icon in address bar (⊕ or computer icon)
   - Android: Look for "Add to Home Screen" banner at bottom of screen
3. **Install the app**:
   - Click the install icon/button
   - Confirm installation in the dialog
4. **Verify installation**:
   - App should open in standalone window (no browser UI)
   - App icon should appear in your app drawer/start menu

### Safari (iOS)

1. **Open the site** in Safari on iPhone/iPad
2. **Tap the Share button** (square with up arrow)
3. **Select "Add to Home Screen"**
4. **Customize the name** if desired (default: "Vegan Campsite")
5. **Tap "Add"** in the top right
6. **Verify installation**:
   - Icon should appear on home screen
   - Tap icon to launch app
   - App should open without Safari UI
   - Status bar should be translucent black

### Safari (macOS)

1. **Open the site** in Safari
2. **Look for install option** in File menu or address bar
3. **Install the app**
4. **Verify installation**:
   - App should open in standalone window
   - App should appear in Applications or Dock

## Expected Results

### Icon Appearance
- **Color**: Green circle (#16a34a)
- **Design**: White tree/pine symbol in center
- **Size**: 192x192 pixels
- **Format**: PNG (converted from SVG)

### App Behavior
- **Display Mode**: Standalone (no browser UI)
- **Theme Color**: Green (#16a34a) - affects status bar and app switcher
- **Background Color**: Light green (#f0fdf4) - shown during app launch
- **Start URL**: Opens to the main cookbook page

### Startup Experience (iOS)
- **Splash Screen**: Light green background with centered green icon
- **Status Bar**: Black translucent style

## Troubleshooting

### "Add to Home Screen" not appearing

**Possible causes:**
- PWA already installed - check home screen/app drawer
- Using private/incognito mode - PWAs require normal browsing mode
- Browser doesn't support PWAs - use Chrome, Edge, or Safari

### Icon not displaying correctly

**Check:**
- Icon should convert from SVG to PNG automatically
- If icon appears broken, check browser console for errors
- Try reinstalling the app (remove from home screen, reinstall)

### App not opening in standalone mode

**Verify:**
- Manifest is loading correctly (check browser DevTools > Application > Manifest)
- `display: standalone` is set in manifest
- App was installed properly (not just a bookmark)

## Developer Tools Testing

### Chrome/Edge DevTools

1. **Open DevTools** (F12)
2. **Go to Application tab**
3. **Check Manifest section**:
   - Name: "Vegan Campsite Cookbook"
   - Short name: "Vegan Campsite"
   - Start URL: Current page URL
   - Display: standalone
   - Theme color: #16a34a
   - Icons: 192x192 PNG
4. **Check Service Workers** (optional - we don't use one yet)
5. **Test install**:
   - Click "Update" to reload manifest
   - Use "Add to home screen" link in DevTools

### Safari Web Inspector

1. **Enable Developer menu**: Safari > Preferences > Advanced > Show Develop menu
2. **Open Web Inspector**: Develop > Show Web Inspector
3. **Check console** for "PWA install prompt available!" message
4. **Verify icons** load correctly

## Validation Checklist

- [ ] PWA meta tags present in HTML `<head>`
- [ ] SVG icon loads correctly (visible in browser tab)
- [ ] Console logs "PWA install prompt available!"
- [ ] Manifest generates with correct metadata
- [ ] Icon converts from SVG to PNG successfully
- [ ] iOS startup image generates correctly
- [ ] Install prompt appears in supported browsers
- [ ] App installs successfully
- [ ] App opens in standalone mode (no browser UI)
- [ ] App icon displays correctly on home screen
- [ ] Theme color applied to status bar/app switcher
- [ ] App works offline after initial CDN loads

## Notes

- **Single File Architecture**: All PWA functionality is embedded in index.html
- **No Service Worker**: Currently relies on browser caching for offline support
- **CDN Dependencies**: Alpine.js, Tailwind CSS, and Bootstrap Icons load from CDNs
- **Offline Capability**: Site works offline after initial load (CDNs cache resources)
- **Cross-Platform**: Works on iOS, Android, macOS, Windows, Linux, and ChromeOS

## Future Enhancements

Possible improvements for better PWA support:
- Add service worker for true offline functionality
- Implement background sync for offline changes
- Add push notifications for new recipes
- Create multiple icon sizes (512x512, etc.)
- Add screenshots for install prompt
- Implement update notification system
