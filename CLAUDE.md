# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

TravianZ is a PHP/MySQL browser-based strategy game (Travian clone). The project is a legacy PHP codebase with some modern namespace-based classes.

## Common Commands

### Development with Docker

```bash
# Start development environment
docker-compose up -d

# View logs
docker-compose logs -f web

# Stop containers
docker-compose down

# Reset installation (removes database and config)
rm -f var/installed GameEngine/config.php
docker-compose down -v
docker-compose up -d
```

### Manual Setup (without Docker)

1. Set up Apache/PHP 7.0+ and MySQL 5.5+
2. Copy `.env.example` to `.env` and configure database credentials
3. Navigate to `/install` to run the installation wizard
4. Create `var/installed` file after successful installation

### Database Access

- phpMyAdmin available at `http://localhost:8081` (Docker)
- MySQL port: `3306`

## Architecture

### Core Directories

- **`GameEngine/`** - Main game logic (legacy procedural PHP)
  - `Automation.php` - Game automation/tick processing
  - `Battle.php` - Combat calculations
  - `Building.php` - Building construction logic
  - `Database.php` - MySQLi database wrapper implementing `App\Database\IDbConnection`
  - `Units.php` - Unit data and calculations
  - `Lang/` - Language files (en.php, it.php, zh_tw.php)

- **`src/`** - Modern PHP classes with `App\` namespace
  - `Database/IDbConnection.php` - Database interface
  - `Entity/User.php` - User entity
  - `Utils/AccessLogger.php` - Request logging
  - `Utils/DateTime.php` - Date utilities
  - `Utils/Math.php` - Math utilities

- **`Templates/`** - HTML templates (.tpl files)
- **`Admin/`** - Admin panel backend
- **`Security/`** - Security class (Security.class.php)
- **`install/`** - Installation wizard

### Entry Points

- `index.php` - Landing page (redirects to install if not configured)
- `login.php` - User login
- `dorf1.php`, `dorf2.php`, `dorf3.php` - Village views
- `karte.php` - Map view

### Configuration

Configuration is generated during installation and stored in `GameEngine/config.php`. Key settings include database credentials, server name, and game parameters.

### Namespace Convention

The `App\` namespace is used for modern classes in `src/`. The custom autoloader (`autoloader.php`) handles loading these classes.

## Development Notes

- No formal test suite exists (no PHPUnit)
- The codebase has ~400 MySQL queries per page refresh (legacy performance characteristics)
- Uses mysqli extension for database operations
- Session management in `GameEngine/Session.php`
- Language system in `GameEngine/Lang/`
