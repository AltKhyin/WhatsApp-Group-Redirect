# WhatsApp Group Redirect Plugin for WordPress

A WordPress plugin that reliably redirects form submissions to WhatsApp groups, forcing the mobile app to open instead of WhatsApp Web.

## Problem It Solves

When you redirect users to a WhatsApp group link (`https://chat.whatsapp.com/CODE`), browsers typically open WhatsApp Web instead of the mobile app. This plugin solves that problem using a landing page with JavaScript that attempts multiple deep linking strategies to force the app to open.

## How It Works

The plugin uses a **two-pronged approach** to maximize success:

1. **Custom URL Scheme** (`whatsapp://chat?code=CODE`) - Forces app opening
2. **Universal Link Fallback** (`https://chat.whatsapp.com/CODE`) - Browser fallback
3. **Manual Button** - User can click if automatic methods fail

This approach works because:
- HTTP 302 redirects to universal links don't trigger mobile OS app interception
- JavaScript-initiated navigation gives the OS a chance to intercept and open the app
- Multiple attempts increase success rate across different browsers and devices

## Features

- Creates custom redirect endpoint (e.g., `/whatsapp-redirect/`)
- Landing page with automatic app opening attempts
- Lead capture and storage (name, email, phone, timestamp, user agent, IP)
- Admin panel to view and export leads as CSV
- Settings page to configure WhatsApp group link and redirect URL
- Works with Elementor forms and any form that supports redirect URLs
- Compatible with WordPress Block Themes (FSE)
- Portuguese UI on landing page (easily customizable)

## Installation

1. Download `whatsapp-redirect-plugin.php`
2. Upload to `/wp-content/plugins/` directory
3. Activate the plugin through the 'Plugins' menu in WordPress
4. Go to **WhatsApp Leads → Settings** to configure your group link

## Configuration

### Step 1: Get Your WhatsApp Group Link

1. Open your WhatsApp group on your phone
2. Tap the group name at the top
3. Scroll down and tap "Invite via link"
4. Tap "Copy link"
5. You'll get something like: `https://chat.whatsapp.com/ABC123xyz`

### Step 2: Configure Plugin Settings

1. In WordPress admin, go to **WhatsApp Leads → Settings**
2. Paste your WhatsApp group link
3. Optionally customize the redirect URL path (default: `/whatsapp-redirect/`)
4. Click "Save Settings"
5. Test using the "Test Redirect" button

### Step 3: Set Up Your Form

**For Elementor Forms:**

1. Edit your form in Elementor
2. Go to **Form → Actions After Submit**
3. Add "Redirect" action
4. Set redirect URL to:
```
https://yourdomain.com/whatsapp-redirect/?name=[field id="name"]&email=[field id="email"]&phone=[field id="phone"]
```

**Important:** Replace `name`, `email`, `phone` with your actual field IDs from Elementor.

**For Other Forms:**

Any form that supports redirect URLs will work. Just use the format:
```
https://yourdomain.com/whatsapp-redirect/?name=NAME&email=EMAIL&phone=PHONE
```

## Finding Elementor Field IDs

1. Edit your page in Elementor
2. Click on your form widget
3. Go to "Form Fields" in the left panel
4. For each field, look for the "ID" setting
5. Note down the exact IDs (e.g., `nome`, `email`, `telefone`)

## Lead Tracking

The plugin automatically captures:
- Name
- Email
- Phone
- Timestamp
- User Agent
- IP Address
- Source

View leads in **WhatsApp Leads** admin menu. Export as CSV anytime.

## Customization

### Change Landing Page Text

Edit lines 203-204 in the plugin file:

```php
<h1>Entrando no Grupo...</h1>
<p>Abrindo WhatsApp...</p>
```

Change to your language or custom message.

### Change Landing Page Colors

Edit the CSS gradient on line 139:

```css
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
```

### Add Webhook Integration

Uncomment line 92 and configure `whatsapp_redirect_trigger_webhook()` function (lines 339-358) to send leads to Make.com, Zapier, or any webhook service.

## Troubleshooting

### Redirect Not Working (404 Error)

Go to **Settings → Permalinks** in WordPress and click "Save Changes" to flush rewrite rules.

### App Not Opening

- Test on different browsers (some in-app browsers like Instagram's are more restrictive)
- Make sure WhatsApp is installed on the device
- Desktop users will always get WhatsApp Web (expected behavior)

### Form Not Passing Data

- Check that your field IDs match exactly (case-sensitive)
- Test with hardcoded values first: `?name=Test&email=test@test.com&phone=123`
- Check browser Network tab to see what URL is actually being called

### Redirect Goes to Homepage

This usually means:
- Field IDs don't match (Elementor leaves placeholders in URL, WordPress rejects invalid URLs)
- Need to flush permalinks

## Technical Details

### Why Not Just Use HTTP 302 Redirect?

HTTP 302 redirects to universal links don't trigger mobile OS app interception. The OS only checks for app handlers when:
1. The URL is part of a **user-initiated** navigation (click)
2. The navigation is the **first** navigation, not reached via redirect

By showing a landing page with JavaScript, we restore the OS's ability to intercept and prompt for app opening.

### Browser Compatibility

- **Mobile Safari (iOS)**: Works excellently
- **Chrome Mobile (Android)**: Works excellently
- **Samsung Internet**: Works well
- **Instagram In-App Browser**: Limited (shows fallback button)
- **Facebook In-App Browser**: Limited (shows fallback button)
- **Desktop Browsers**: Opens WhatsApp Web (expected)

## License

GPL v2 or later

## Credits

Created to solve the universal link limitation problem with WhatsApp group redirects.

## Support

For issues, questions, or improvements, please open an issue on GitHub.

## Changelog

### 1.0.0
- Initial release
- Custom redirect endpoint
- Landing page with deep linking
- Lead tracking and CSV export
- Settings page for configuration
- Compatible with Elementor forms
