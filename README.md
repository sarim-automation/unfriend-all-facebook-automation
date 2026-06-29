# Unfriend All Facebook Automation

A lightweight JavaScript automation script that demonstrates browser automation by navigating a Facebook Friends list and interacting with the interface using modern DOM APIs.

> **Educational Purpose Only**
>
> This project is intended solely for educational, research, and learning purposes. It demonstrates browser automation techniques, DOM manipulation, asynchronous JavaScript, and UI interaction. This repository is **not affiliated with, endorsed by, or supported by Meta or Facebook.**

---

# Overview

This script automates repetitive interactions on the Facebook Friends page by locating available friend actions, navigating menus, confirming selections, and continuing until the visible list has been processed.

The project is designed to showcase practical browser automation concepts rather than encourage misuse of any online service.

---

# Features

* Pure JavaScript
* No external libraries or dependencies
* Runs directly from the browser console
* Automatic scrolling
* Dynamic element detection
* Confirmation dialog handling
* Randomized delays between actions
* Progress logging in the browser console
* Stops automatically after processing all visible entries
* Easy to modify and extend

---

# Requirements

* Google Chrome
* Microsoft Edge
* Brave Browser
* Opera
* Any modern Chromium-based browser

No installation required.

---

# Usage

1. Open your Facebook Friends page.
2. Open Developer Tools (`F12` or `Ctrl + Shift + I`).
3. Select the **Console** tab.
4. Paste the script into the console.
5. Press **Enter**.

The script will begin processing automatically.

---

# How It Works

The automation performs the following steps:

1. Scans the page for friend action buttons.
2. Opens the available action menu.
3. Searches for the **Unfriend** option.
4. Confirms the action if prompted.
5. Waits for a randomized delay.
6. Scrolls to load additional friends.
7. Repeats until no more entries are found.

---

# Project Structure

```text
Unfriend-All-Facebook-Automation/
│
├── README.md
├── script.js
└── LICENSE
```

---

# Disclaimer

This project is provided **"AS IS"**, without warranty of any kind, express or implied.

By using this software you acknowledge that:

* You are solely responsible for how you use this project.
* The author accepts no responsibility for account restrictions, suspensions, bans, loss of access, data loss, or any damages arising from the use of this software.
* You are responsible for ensuring compliance with all applicable laws, regulations, and the Terms of Service of any platform on which this software is used.
* This project is intended only as an educational demonstration of browser automation techniques.
* The author does not encourage or endorse automation that violates third-party platform policies.

---

# Warning

Websites frequently update their user interfaces.

As a result:

* The script may stop working without notice.
* Selectors may require updates.
* Automation on third-party platforms may trigger security checks.
* Your account may be temporarily or permanently restricted if the platform determines that automated activity violates its policies.

Use this project entirely at your own risk.

---

# Contributing

Contributions are welcome.

Bug reports, feature requests, documentation improvements, and pull requests are appreciated.

Please open an issue before submitting major changes.

---

# License

This project is licensed under the MIT License.

You are free to use, modify, and distribute this software under the terms of the MIT License. See the `LICENSE` file for complete details.
