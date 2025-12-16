# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Petit Note V is a PHP-based drawing board (oekaki keijiban) application that supports multiple drawing tools (PaintBBS NEO, tegaki.js, ChickenPaint, Klecks, and Axnos Paint). It's a fork of Petit Note by satopian that uses SQLite for data storage and includes both traditional PHP templates and a Vue.js-based template.

## Technology Stack

- **Backend**: PHP 7.4+ (PHP 8.1+ recommended)
- **Database**: SQLite (database files stored in `petitnote/log/`)
- **Frontend**:
  - Basic template: HTML with server-side PHP templating
  - Mono Vue template: Vue.js 3 with TypeScript and Vite
- **Drawing Apps**: Multiple embedded JavaScript/WebAssembly drawing applications
- **Testing**: PHPUnit 9

## Core Architecture

### Entry Point and Routing

- `petitnote/index.php` - Main entry point that handles all requests
- `petitnote/functions.php` - Core PHP functions library (20MB+ file with extensive functionality)
- Routing is handled through query parameters (`mode`, `page`, `resno`, etc.)

### Configuration

- `petitnote/config.php` - Main configuration file (created from `config.example.php`)
- Configuration includes:
  - Admin passwords (two-factor: `$admin_pass` and `$second_pass`)
  - Database name (`$database_name`)
  - Feature flags (drawing tools, SNS sharing, Misskey integration)
  - Spam prevention settings
  - Display settings and limits
  - Image handling parameters

### Data Storage

- SQLite database: `petitnote/log/{database_name}.db`
- Images: `petitnote/src/`
- Thumbnails: `petitnote/thumbnail/`
- Temporary files: `petitnote/temp/`

### Template System

- Templates located in `petitnote/template/`
- Two template systems:
  - `basic/` - Server-side PHP templating with `.html` files that embed PHP variables
  - `mono_vue/` - Vue.js SPA (currently in development, has node_modules)

### Key Modules (`.inc.php` files)

- `save.inc.php` - Handles image saving from drawing applications
- `search.inc.php` - Search functionality
- `misskey_note.inc.php` - Misskey social network integration
- `sns_share.inc.php` - SNS sharing functionality
- `noticemail.inc.php` - Email notifications
- `thumbnail_gd.inc.php` - Image thumbnail generation using GD library

### Drawing Applications

Located in `petitnote/app/`:

- `neo/` - PaintBBS NEO
- `tegaki/` - tegaki.js
- `chickenpaint/` - ChickenPaint (litaChix)
- `klecks/` - Klecks
- `axnos/` - Axnos Paint

Each application has its own configuration and can be enabled/disabled via config.php.

## Common Development Commands

### PHP Testing

```bash
# Run all tests
vendor/bin/phpunit

# Run specific test file
vendor/bin/phpunit tests/CommonFunctionTest.php

# Run with coverage (if configured)
vendor/bin/phpunit --coverage-html coverage/
```

### Composer Dependencies

```bash
# Install PHP dev dependencies (PHPUnit, etc.)
composer install

# Update dependencies
composer update
```

### Vue.js Template Development (mono_vue)

```bash
cd petitnote/template/mono_vue

# Install dependencies (uses pnpm)
pnpm install

# Development server
pnpm dev

# Build for production
pnpm build

# Preview production build
pnpm preview
```

## Important Development Notes

### Security Considerations

- Admin passwords must be changed from defaults in `config.php`
- The application includes extensive spam prevention:
  - Japanese language filter
  - Bad string/URL/name/host filtering
  - Reverse DNS checking
  - Session-based ban caching
- CSRF protection through session tokens
- Password encryption for paint screen parameters
- XSS prevention through input sanitization functions (`h()`, `s()`, `t()`, `com()`)

### Database Structure

- Uses SQLite with PDO
- Database path defined as `LOG_DIR.$database_name.".db"`
- Thread-based structure with parent posts and replies (resto field)
- Automatic cleanup when `$max_log` limit is reached

### Image Processing

- Supports PNG, WEBP, and animated formats
- Automatic WEBP conversion for large files
- Thumbnail generation when images exceed display size limits
- GD library used for image manipulation
- Canvas size limits defined in config: `$pmin_w`, `$pmin_h`, `$pmax_w`, `$pmax_h`

### Session Management

- Custom session name configurable via `$session_name`
- Multiple session modes:
  - Admin mode (`$_SESSION['adminpost']`)
  - Admin delete mode (`$_SESSION['admindel']`)
  - User delete/edit mode (`$_SESSION['userdel']`)
  - Aikotoba (password) authentication (`$_SESSION['aikotoba']`)

### Template Variables

Templates in `basic/` directory receive PHP variables directly:

- `$threads` - Array of thread objects
- `$resno` - Current thread number for reply view
- `$boardname` - Board name
- `$admin_pass` - Used for admin authentication forms
- `$en` - English language flag

### Misskey Integration

- Direct posting to Misskey instances
- Multiple server support configured in `$misskey_servers`
- Uses `connect_misskey_api.php` for API communication
- Server selection UI in templates

## Testing Guidelines

### PHPUnit Tests

- Test files in `tests/` directory
- Tests require `petitnote/functions.php` autoloaded (configured in `composer.json`)
- Coverage configuration targets `petitnote/` directory
- Common functions tested: `t()`, `s()`, `h()`, `com()`, `auto_link()`, `md_link()`

### Manual Testing

When testing changes:

1. Ensure PHP version compatibility (7.4+, preferably 8.1+)
2. Test with SQLite database creation/migration
3. Verify all drawing applications load correctly
4. Check image upload and thumbnail generation
5. Test admin authentication flows
6. Verify spam filters don't block legitimate content

## File Permissions

The application automatically sets file permissions:

- Log files: 0600 (`PERMISSION_FOR_LOG`)
- Destination files: 0606 (`PERMISSION_FOR_DEST`)
- Do not manually change permissions as it may break functionality

## Known Quirks

1. **Large functions.php**: The main functions file is very large (35K+ lines). When editing, work on specific functions rather than reading the entire file.

2. **Template System**: The `basic/` template uses `.html` extension but contains PHP. These are included via `include` statements.

3. **.htaccess Files**: Included for security but may cause issues on some servers. Can be safely removed if causing errors, but only as a last resort.

4. **Japanese Language**: Core audience is Japanese, but English support exists via `$en` flag based on browser language detection.

5. **Multiple Drawing App Dependencies**: Each drawing app in `petitnote/app/` is a separate project with its own license and structure.

## Git Workflow

Recent commits show:

- Bug fixes for thread display issues
- jQuery integration work
- Config existence checking
- Template fixes in `catalog_images_loop.html` and `threads_loop.html`

Current staged changes:

- `M petitnote/template/basic/parts/catalog_images_loop.html`
- `M petitnote/template/basic/parts/threads_loop.html`
- `?? petitnote/test.php`

Main branch is `master`.
