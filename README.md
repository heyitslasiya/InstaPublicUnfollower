#InstaPublicUnfollower

Automatically unfollow Instagram public accounts safely and efficiently.

⚡ Features

Unfollows only public Instagram accounts.

Skips private accounts automatically.

Limits unfollows to a maximum per day to reduce the risk of blocks.

Introduces random delays to mimic human behavior.

Can be manually stopped anytime during execution.

Tracks daily unfollow count in localStorage.

🛠️ Usage

1. Open Instagram in your browser and log in.


2. Go to your Following list.


3. Open the browser console (F12 or Ctrl+Shift+I → Console).


4. Paste the script into the console.


5. Press Enter to start.


6. To stop the script anytime:

window.STOP_UNFOLLOW = true;


⚙️ Configuration

Maximum unfollows per day:

const MAX_PER_DAY = 400;

Delay between unfollows:

const PAUSE_DURATION = 10 * 60 * 1000; // 10 minutes

Random delay for clicks:

const randomDelay = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;


You can adjust these values to suit your needs.


⚠️ Warnings

Use responsibly. Automating unfollows may trigger temporary blocks or restrictions on Instagram.

Only tested on public accounts. Private accounts are skipped automatically.

Instagram UI changes may break the script. Check for updated selectors if it stops working.



📌 Notes

Progress is tracked per day in your browser’s localStorage.

Logs detailed info in the console for skipped accounts and daily limits.



💡 Ideas for Improvements

Add batch unfollowing for faster sessions.

Add export logs to a file for tracking.

Enhance error handling for modal changes and UI updates.

👨‍💻 Code

(async function () {   const MAX_PER_DAY = 400;   const PAUSE_DURATION = 10 * 60 * 1000; // 10 minutes   const delay = (ms) => new Promise((res) => setTimeout(res, ms));   const randomDelay = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;    const today = new Date().toISOString().split("T")[0];   const key = unfollowed_${today};   let unfollowed = parseInt(localStorage.getItem(key) || "0");    if (unfollowed >= MAX_PER_DAY) {     console.log(🛑 Already reached ${MAX_PER_DAY} unfollows today.);     return;   }    window.STOP_UNFOLLOW = false;   console.log(🚀 Starting session: ${unfollowed}/${MAX_PER_DAY});    // Keep track of indexes of buttons to skip (private or problematic accounts)   const skippedIndexes = new Set();    const getFollowingButtons = () =>     [...document.querySelectorAll("button")]       .filter(btn => btn.innerText === "Following");    while (unfollowed < MAX_PER_DAY) {     if (window.STOP_UNFOLLOW) {       console.log("🛑 Unfollow process manually stopped.");       break;     }      const buttons = getFollowingButtons();      // Filter out buttons that we've already skipped     const availableIndexes = buttons       .map((btn, idx) => idx)       .filter(idx => !skippedIndexes.has(idx));      if (availableIndexes.length === 0) {       console.log("❗ No more unfollowable 'Following' buttons found.");       break;     }      const idxToClick = availableIndexes[0];     const btn = buttons[idxToClick];      btn.scrollIntoView({ behavior: "smooth", block: "center" });     await delay(randomDelay(800, 1500));     btn.click(); // Open unfollow modal      await delay(1500); // Let modal appear      // Check for private account warning     const modalText = [...document.querySelectorAll("div")]       .map(div => div.innerText.trim())       .find(text => text.includes("you'll have to request to follow"));      if (modalText) {       console.log(🚫 Skipped private account at index ${idxToClick} (request warning shown).);       skippedIndexes.add(idxToClick); // Mark this button as skipped to avoid retrying       // Close modal       const cancelBtn = [...document.querySelectorAll("button")].find(b => b.innerText === "Cancel");       if (cancelBtn) cancelBtn.click();       await delay(1000);       window.scrollBy(0, 100);       continue;     }      // Proceed to unfollow     const confirmBtn = [...document.querySelectorAll("button")]       .find(b => b.innerText === "Unfollow");      if (!confirmBtn) {       console.log(⚠️ Confirm button not found at index ${idxToClick}. Skipping and marking as skipped.);       skippedIndexes.add(idxToClick); // Mark as skipped       // Close modal if possible       const cancelBtn = [...document.querySelectorAll("button")].find(b => b.innerText === "Cancel");       if (cancelBtn) cancelBtn.click();       await delay(1000);       continue;     }      confirmBtn.click(); // Unfollow     unfollowed++;     localStorage.setItem(key, unfollowed);     console.log(✅ Unfollowed public account #${unfollowed} at index ${idxToClick});      await delay(2000); // Wait for UI update     window.scrollBy(0, randomDelay(100, 300));      console.log("⏳ Waiting 10 minutes before next unfollow...");     await delay(PAUSE_DURATION);   }    console.log(🏁 Finished. Total unfollowed today: ${unfollowed}); })();
