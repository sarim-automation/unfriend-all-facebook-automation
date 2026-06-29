
# Unfriend All Facebook Automation

A JavaScript automation script that demonstrates browser DOM interaction and UI automation techniques by navigating a Facebook friends list and automating repetitive actions.

> ** Educational Purpose Only**
>
> This project is intended **solely for educational, research, and learning purposes**. It is designed to demonstrate browser automation concepts such as:
>
> * DOM traversal
> * Element detection
> * UI interaction
> * Asynchronous JavaScript
> * Browser automation techniques
>
> This repository is **not** intended to encourage misuse or violation of any website's Terms of Service.

---

## Features

* Automatically scans visible friend entries.
* Detects the appropriate action menu.
* Attempts to locate the **Unfriend** option.
* Handles confirmation dialogs.
* Uses randomized delays to simulate normal interaction.
* Automatically scrolls until all visible friends have been processed.
* Console logging for progress tracking.

---

## Usage

1. Open your Facebook **Friends** page. (https://www.facebook.com/friends/list)
2. Press **F12** (or `Ctrl + Shift + I`) to open Developer Tools.
3. Navigate to the **Console** tab.
4. Paste the script below.
5. Press **Enter** and let it run.

---

## Script

```javascript
// Paste your script here

async function smartUnfriendLoop() {
  const delay = (ms) => new Promise((res) => setTimeout(res, ms));
  const randomDelay = (min, max) => delay(Math.floor(Math.random() * (max - min + 1)) + min);

  let processed = new Set();
  let consecutiveFails = 0;

  console.log("🚀 Starting robust unfriend loop...");

  while (true) {
    // Get all potential clickable elements
    const allButtons = Array.from(document.querySelectorAll('div[role="button"], button, a, span[role="button"], div[aria-label]'));
    
    // 1. Prioritize the "More" (three dots) menu, as Facebook often hides "Unfriend" there
    let menuButtons = allButtons.filter(btn => {
      if (processed.has(btn)) return false;
      const aria = (btn.getAttribute('aria-label') || '').toLowerCase();
      // Look for "More", "Action options", or "See more options"
      const isMoreMenu = aria.includes('more') || aria.includes('action options') || aria.includes('see more');
      const rect = btn.getBoundingClientRect();
      // The three-dots button is usually very small (around 30x30 to 50x50 pixels)
      const isSmallButton = rect.width > 0 && rect.width < 80 && rect.height > 0 && rect.height < 80; 
      return isMoreMenu && isSmallButton;
    });

    // 2. Fallback to the "Friends" button if "More" isn't found
    if (menuButtons.length === 0) {
       menuButtons = allButtons.filter(btn => {
          if (processed.has(btn)) return false;
          const aria = (btn.getAttribute('aria-label') || '').toLowerCase();
          const text = btn.textContent.trim().toLowerCase();
          const isFriendsButton = text === 'friends' || aria.includes('friends');
          const rect = btn.getBoundingClientRect();
          const isSmallButton = rect.width > 0 && rect.width < 150 && rect.height > 0 && rect.height < 100; 
          return isFriendsButton && isSmallButton;
       });
    }

    // If no buttons found, scroll down to load more friends
    if (menuButtons.length === 0) {
      window.scrollBy(0, 800);
      await randomDelay(1500, 2500);
      
      consecutiveFails++;
      if (consecutiveFails > 5) {
        console.log("✅ All done! Looks like you’ve unfriended everyone or reached the end of the list.");
        break;
      }
      continue;
    }
    
    consecutiveFails = 0;

    // Process the visible friend buttons
    for (const btn of menuButtons) {
      processed.add(btn);
      btn.scrollIntoView({ block: "center" });
      await randomDelay(500, 1000);

      const btnLabel = btn.getAttribute('aria-label') || btn.textContent.trim();
      console.log(`🖱️ Clicking menu button: "${btnLabel}"`);
      
      // Click the button to open the menu
      btn.click();
      await randomDelay(1000, 1500);

      // 3. Find the "Unfriend" option in the menu.
      // Menus are often rendered in a portal at the end of <body>.
      const candidates = Array.from(document.querySelectorAll('div[role="menuitem"], span[role="menuitem"], a[role="menuitem"], div[role="button"], button, a, span'));
      
      const unfriendBtn = candidates.find(el => {
        const text = el.textContent.trim().toLowerCase();
        const aria = (el.getAttribute('aria-label') || '').toLowerCase();
        const rect = el.getBoundingClientRect();
        // It should be visible and not a massive container
        const isReasonableSize = rect.width > 0 && rect.width < 400 && rect.height > 0 && rect.height < 100;
        return isReasonableSize && (
            text.includes('unfriend') || text.includes('remove friend') || 
            aria.includes('unfriend') || aria.includes('remove friend')
        );
      });

      if (unfriendBtn) {
        unfriendBtn.click();
        await randomDelay(1000, 1500);

        // 4. Find the confirmation button in the popup.
        const allButtonsOnPage = Array.from(document.querySelectorAll('div[role="button"], button, a'));
        let confirmBtn = allButtonsOnPage.find(el => {
          // Check if the button is inside a modal/dialog
          const modal = el.closest('div[role="dialog"]') || el.closest('div[aria-modal="true"]') || el.closest('div[data-type="modal"]');
          if (modal) {
            const text = el.textContent.trim().toLowerCase();
            const aria = (el.getAttribute('aria-label') || '').toLowerCase();
            const rect = el.getBoundingClientRect();
            const isReasonableSize = rect.width > 0 && rect.width < 300 && rect.height > 0 && rect.height < 100;
            return isReasonableSize && (
                text.includes('confirm') || text.includes('unfriend') || text.includes('remove') ||
                aria.includes('confirm') || aria.includes('unfriend') || aria.includes('remove')
            );
          }
          return false;
        });

        // Fallback: if no modal found, look for any visible button with "Confirm" or "Unfriend"
        if (!confirmBtn) {
            confirmBtn = allButtonsOnPage.find(el => {
                const text = el.textContent.trim().toLowerCase();
                const aria = (el.getAttribute('aria-label') || '').toLowerCase();
                const rect = el.getBoundingClientRect();
                const isVisible = rect.width > 0 && rect.height > 0 && rect.top >= 0 && rect.top < window.innerHeight;
                const isReasonableSize = rect.width < 300 && rect.height < 100;
                return isVisible && isReasonableSize && (
                    text === 'confirm' || text === 'unfriend' || text === 'remove' ||
                    aria === 'confirm' || aria === 'unfriend' || aria === 'remove'
                );
            });
        }

        if (confirmBtn) {
          confirmBtn.click();
          console.log(`👋 Successfully unfriended! Total processed: ${processed.size}`);
          await randomDelay(2000, 3000); 
        } else {
          console.log("⚠️ Could not find the confirm button in the modal. Skipping this friend.");
        }
      } else {
        console.log("⚠️ Could not find the 'Unfriend' menu item. The menu might not have opened correctly. Skipping.");
      }

      // Random delay between friends to avoid Facebook's bot detection
      await randomDelay(2000, 4000); 
    }
  }
}

// Run the loop
smartUnfriendLoop();

```

---

## Disclaimer

By using this software, you acknowledge and agree that:

* This project is provided **"AS IS"**, without any warranty of any kind.
* You are solely responsible for how you use this script.
* The author assumes **no liability** for account restrictions, suspensions, bans, data loss, or any other consequences resulting from its use.
* You are responsible for ensuring that your use complies with all applicable laws and the Terms of Service of any platform on which the script is used.
* The author does **not** encourage, promote, or endorse automation that violates third-party platform policies.

---

## Warning

Automating interactions on third-party websites may violate their Terms of Service.

Using this script could result in:

* Temporary account restrictions
* Permanent account suspension
* Security challenges or verification requests
* Unexpected behavior if the website changes

Use it **at your own risk**.

---

## Contributing

Contributions, bug reports, and improvements are welcome.

If you have ideas to improve the automation logic or make it more resilient to UI changes, feel free to open an issue or submit a pull request.

---

## License

This project is released under the **MIT License**.

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, subject to the conditions of the MIT License.
