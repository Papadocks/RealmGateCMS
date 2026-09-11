# RealmGateCMS

RealmGateCMS is a World of Warcraft server CMS developed from SahtoutCMS, with initial support for **AzerothCore WotLK 3.3.5** and a planned modular architecture.

[Repository](https://github.com/Papadocks/RealmGateCMS) · [Issues](https://github.com/Papadocks/RealmGateCMS/issues) · [Modernization roadmap](RealmGateCMS-Modernization-Roadmap.md)

The project is undergoing modernization. The functionality and setup below describe the inherited implementation; the Laravel architecture and other roadmap features are planned work, not completed capabilities.

SahtoutCMS is the upstream project created by blodyiheb. Its provenance and original MIT copyright notice are preserved in [NOTICE.md](NOTICE.md) and [LICENSE](LICENSE).

---

# ✨ Features

## 👤 Account Management

* SRP6 registration and authentication
* Email activation
* Resend activation email
* Secure login
* reCAPTCHA support
* Forgot password
* Password reset
* Account dashboard
* Password management
* Email management
* Character information
* Character statistics

---

# 🛡️ Administration Panel

RealmGateCMS provides a centralized administration panel for managing your website and server-related content.

## 👥 Users

* Website account management
* User roles
* Email management
* User information
* Tokens and points

## 🎮 In-Game Accounts

* View game accounts
* Ban / unban accounts
* Manage GM levels
* Account information

## 🧙 Characters

* Character information
* Character statistics
* Level
* Gold
* Teleportation
* Character management

## 📰 Content Management

* Create news
* Edit news
* Delete news
* Manage shop products
* Manage shop services

## ⚔️ SOAP Command Executor

Execute AzerothCore GM commands directly from the administration panel through SOAP.

---

# ⚙️ Administration Settings

RealmGateCMS provides configurable settings for the main components of the website.

### General

* Website logo
* Social media links
* General website configuration

### SMTP

* SMTP server configuration
* Email activation
* Password recovery emails

### reCAPTCHA

* Site key
* Secret key
* Enable / disable reCAPTCHA

### Realm

* Realm name
* Realm IP
* Realm port
* Realm logo

### SOAP

* SOAP connection
* SOAP credentials
* GM command configuration

### Voting

* Voting websites
* Vote rewards
* Vote cooldowns
* Point rewards

---

# 🛒 Shop System

RealmGateCMS includes a dynamic shop system managed directly from the administration panel.

## 🧰 Item Shop

* Equipment
* Mounts
* Pets
* WoW item information
* WoW-style item tooltips

## 🛠️ Character Services

* Character rename
* Faction change
* Level boost
* Gold

## 💰 Currency

* Tokens / points
* Currency management
* Shop pricing
* Player balances

---

# 🗳️ Voting System

Reward your players for supporting your server through external voting websites.

Features include:

* Multiple voting websites
* Vote rewards
* Voting cooldowns
* Vote history
* Point rewards
* Configurable voting settings

---

# ⚔️ Armory

RealmGateCMS includes an Armory system for displaying player and character information.

## 🏆 Top Players

Display top players based on:

* Level
* PvP kills
* Race
* Class
* Faction
* Guild

## 🏟️ Arena Rankings

Arena rankings for:

* 2v2
* 3v3
* 5v5

Including:

* Wins
* Losses
* Win rate
* Rating

## 🧙 Character Armory

* Equipment inspection
* Character statistics
* Items
* 3D model support

---

# 🎮 World of Warcraft Features

## 🌍 Realm

* Online / offline status
* Online players
* Server uptime
* Realm information

## 🧰 Items

* WoW-style item tooltips
* Item information
* Item data retrieved from the game database

## 🧙 Characters

* Character information
* Equipment inspection
* Character statistics
* 3D models

## 🎨 Interface

* Modern WoW-inspired design
* Responsive interface
* Dark fantasy aesthetic
* Discord widget
* Mobile-friendly layout

---

# 🔐 Security

RealmGateCMS includes several security-focused features, including:

* SRP6 authentication
* Password reset protection
* Email activation
* reCAPTCHA
* Role-based administration
* Protected administration routes
* Secure database interactions
* Configurable authentication settings

> ⚠️ Always keep your PHP installation, web server, AzerothCore server, and dependencies updated and properly configured.

---

# 💻 Requirements

## Upstream Reference Environment

The following environment was documented by SahtoutCMS; it has not been independently revalidated for this fork.

| Component | Version     |
| --------- | ----------- |
| OS        | Windows x64 |
| XAMPP     | 8.2.12      |
| PHP       | 8.2.12      |
| Apache    | 2.4.58      |
| MariaDB   | 10.4.32     |
### Additional Requirements
 Node.js
 npm
### Other Platforms

RealmGateCMS should also work on Linux-based Apache environments.

XAMPP is **not required** and is mainly recommended for easy local development and testing.

---

# 🧩 Required PHP Extensions

The following PHP extensions are required:

```text
bcmath
curl
gd
gmp
mbstring
mysqli
openssl
soap
xml
```

You can check your installed PHP extensions with:

```bash
php -m
```

---

# 🎮 Game Server Requirements

RealmGateCMS is designed for:

* **AzerothCore**
* **World of Warcraft WotLK 3.3.5**
* **SOAP enabled**

Your AzerothCore server and databases should already be installed and working before configuring RealmGateCMS.

---

# 🚀 Installation

## 1. Download RealmGateCMS

Clone the repository:

```bash
git clone https://github.com/Papadocks/RealmGateCMS.git
```

Or download the repository as a ZIP from GitHub.

> The `main` branch contains ongoing RealmGateCMS development. Consult the roadmap for planned changes.
# 📦 Dependencies

```bash
composer install
npm install
npm run build
```
---

# 🪟 2. Windows / XAMPP Installation

Extract RealmGateCMS into:

```text
C:\xampp\htdocs\
```

The project root should look similar to:

### ✅ Correct

```text
C:\xampp\htdocs\
├── admin/
├── assets/
├── includes/
├── install/
├── index.php
└── ...
```

### ❌ Incorrect

```text
C:\xampp\htdocs\RealmGateCMS\RealmGateCMS\
```

Make sure the project is not unnecessarily nested inside another directory.

Start:

* Apache
* MySQL / MariaDB

from the XAMPP Control Panel.

> XAMPP is optional and is mainly recommended for local Windows development.

---

# 🐧 3. Linux / Apache Installation

Extract RealmGateCMS into your Apache document root.

A common location is:

```text
/var/www/html/
```

For example:

```text
/var/www/html/
├── admin/
├── assets/
├── includes/
├── install/
├── index.php
└── ...
```

If your Apache configuration uses another document root, place RealmGateCMS inside that directory.

Make sure Apache has permission to read and execute the required project files.

---

# 🗄️ Database Setup

Before running the installer, make sure your **AzerothCore databases are already installed and working**.

RealmGateCMS uses its own website database and connects to the AzerothCore databases.

## Website Database

Import the supplied `sahtout_site` SQL file. This inherited database name and the SQL filenames below are retained for compatibility; they are not the product name.

## AzerothCore Databases

Import the required additional SQL files into their corresponding AzerothCore databases.

For example:

```text
acore_auth_sahtout_site.sql
→ acore_auth

acore_world_armory_spell.sql
→ acore_world
```

> ⚠️ SQL filenames and database requirements may change between releases. Always check the SQL files included with the version you are installing.

Make sure the required databases exist before continuing with the installer.

---

# 🔧 Web Installer

After preparing the project files and databases, open the installer in your browser.

## XAMPP / Local Development

```text
http://localhost/install/
```

## Production Server

```text
http(s)://your-domain.com/install/
```

Replace `your-domain.com` with your actual domain.

The installer will guide you through the required configuration.

---

# ⚙️ Installer Configuration

Depending on the version, the installer may configure:

* Website database
* AzerothCore database connections
* Realm information
* SMTP / email
* reCAPTCHA
* SOAP
* Website settings

Make sure all database credentials and AzerothCore connection information are correct.

---

# 🔐 Post-Installation

## 1. Remove the Installer

After successfully completing the installation, **remove the `install/` directory** from your server.

```text
install/
```

This helps prevent unauthorized access to the installation system.

---

## 2. Configure Your Administrator Account

After installation,Change the Account's Role to admin in sahtout_site ,user_currencies table
log into the **RealmGateCMS Admin Panel** and complete the remaining configuration.

---

# 🛠️ Development

RealmGateCMS development follows the [modernization roadmap](RealmGateCMS-Modernization-Roadmap.md).

Development is focused on:

* 🎨 UI and UX improvements
* 🔐 Security improvements
* ⚙️ Installer improvements
* 🛡️ Administration improvements
* 🌐 Multilingual support
* 🚀 Performance optimization
* 🧹 Code cleanup
* 📱 Responsive design
* 🐛 Bug fixes
* ✨ New features

The project will continue to evolve based on community feedback and development priorities.

---

# 📌 Project Status

RealmGateCMS is an independent fork in modernization. SahtoutCMS V1/V2 are upstream version names, not RealmGateCMS release names. Existing package version fields are inherited metadata and do not certify completion of the planned RealmGateCMS v1.0.

The `main` branch is the development branch. See the [roadmap](RealmGateCMS-Modernization-Roadmap.md) for the intended product direction and rebranding checklist.

---

# 🐛 Bug Reports

If you find a bug, please open a [GitHub issue](https://github.com/Papadocks/RealmGateCMS/issues).

When reporting an issue, provide as much information as possible:

* What happened
* What you expected to happen
* Steps to reproduce the issue
* RealmGateCMS version
* PHP version
* Apache version
* Database version
* AzerothCore version
* Relevant error messages
* Screenshots or logs when applicable

This makes it much easier to investigate and fix the problem.

---

# 🤝 Contributing

Contributions, suggestions, bug reports, and improvements are welcome!

### Contribution Workflow

1. Fork the repository
2. Create a new branch from `main`
3. Make your changes
4. Test your changes
5. Push your branch
6. Open a Pull Request

Please keep changes focused and provide a clear description of what your Pull Request changes.

---

# 🌟 Support RealmGateCMS

Support the project by starring the [repository](https://github.com/Papadocks/RealmGateCMS), reporting issues, improving documentation or contributing focused pull requests.

---

# 📜 License

RealmGateCMS is released under the **MIT License**.

See [LICENSE](LICENSE) and [NOTICE.md](NOTICE.md) for the original copyright notice and upstream attribution.

---
# 📸 Upstream Screenshots

These screenshots were inherited from SahtoutCMS and show the upstream interface and branding. They are historical references, not verified screenshots of the RealmGateCMS rebrand. Replacement screenshots are tracked in the roadmap.
<img width="1886" height="900" alt="1" src="https://github.com/user-attachments/assets/f914ed6e-d48c-463a-b528-7bd5e2622357" />
<div align="center">
<img width="789" height="906" alt="2" src="https://github.com/user-attachments/assets/f3f11526-a620-4527-9809-4fdb3d307da4" />
</div>
<img width="1875" height="871" alt="3" src="https://github.com/user-attachments/assets/bd9670a7-b8da-4a18-b4a0-a37a2f0cbae5" />
<div align="center">
<img width="671" height="876" alt="4" src="https://github.com/user-attachments/assets/a09a8e37-46a6-43f8-b33f-e1d43263e3ac" />
</div>
<img width="1295" height="909" alt="5" src="https://github.com/user-attachments/assets/5d551112-2c1a-47eb-acd6-45a9a88d2216" /><img width="833" height="299" alt="support-button_original" src="https://github.com/user-attachments/assets/9dd2f7de-cc6f-41e3-9b6e-0f2723413e1b" />
<img width="1075" height="660" alt="6" src="https://github.com/user-attachments/assets/36f854bb-75f6-4d71-bbda-e00cbcd368ea" />
<img width="910" height="601" alt="7" src="https://github.com/user-attachments/assets/b2e78a6e-998a-41bd-bfc1-515f71d06c9b" />
<img width="1294" height="770" alt="8" src="https://github.com/user-attachments/assets/87488f52-a7f3-41d8-87e5-bc7c17583e61" />
<img width="1299" height="909" alt="9" src="https://github.com/user-attachments/assets/39f84e8e-c759-4a88-a083-f9fb5d3b62e4" />
<div align="center">
<img width="826" height="635" alt="10" src="https://github.com/user-attachments/assets/e64b5e45-7214-405b-a196-741586a8c790" />
</div>
<img width="1047" height="615" alt="11" src="https://github.com/user-attachments/assets/3b8cb94c-e7e3-4377-b1f9-ce08a107857f" />
<img width="1078" height="898" alt="12" src="https://github.com/user-attachments/assets/15fb5e45-a0dd-47bb-b6c4-d190a9ec98a4" />
<img width="1606" height="936" alt="13" src="https://github.com/user-attachments/assets/ae3d6c41-e03f-492a-8a58-e52b65d66ed8" />
<img width="1343" height="796" alt="14" src="https://github.com/user-attachments/assets/dd1c12ad-684f-49ab-bc06-6ee4b948865a" />
<img width="1145" height="902" alt="15" src="https://github.com/user-attachments/assets/d4b328d2-e3aa-40e5-aa57-64466ff03464" />
<img width="1429" height="733" alt="16" src="https://github.com/user-attachments/assets/286ea831-0f6e-4c1f-8951-de0410141461" />
<img width="1171" height="907" alt="17" src="https://github.com/user-attachments/assets/548e7ded-7ca3-43f6-9172-7ddd469056d5" />
<img width="1136" height="379" alt="18" src="https://github.com/user-attachments/assets/3e504ac4-9cdc-4ac9-b344-ae20750004cb" />
<div align="center">
<img width="812" height="868" alt="19" src="https://github.com/user-attachments/assets/4a58c291-33dd-4e5d-8b63-ded79e08af02" />

</div>

---

# 🔗 Links

| Resource | Link |
| --- | --- |
| RealmGateCMS repository | [Papadocks/RealmGateCMS](https://github.com/Papadocks/RealmGateCMS) |
| RealmGateCMS issues | [Issue tracker](https://github.com/Papadocks/RealmGateCMS/issues) |
| Product roadmap | [Modernization roadmap](RealmGateCMS-Modernization-Roadmap.md) |
| Upstream project | [SahtoutCMS by blodyiheb](https://github.com/blodyiheb/SahtoutCMS) |
| Upstream legacy version | [SahtoutCMS V1](https://github.com/blodyiheb/SahtoutCMS/tree/v1-legacy) |

---

RealmGateCMS is derived from SahtoutCMS, originally created by **blodyiheb**. See [NOTICE.md](NOTICE.md) for attribution.
