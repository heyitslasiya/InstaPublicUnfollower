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
