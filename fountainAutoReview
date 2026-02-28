/**
 * Fountain Multi-User Auto-Reviewer
 * Matches specific authors and clicks Yes/Save, otherwise Skips.
 */

(function startMultiUserReview() {
    // List of users to automatically accept
    const TARGET_USERS = [
        "Nettime Sujata",
        "Borhan",
        "RDasgupta",
        "নিয়াজ ইসলাম",
        "মোঃ মালেক ইসলাম",
        "কমলেশ মন্ডল"
    ];

    const ACTION_DELAY = 2500; // 2.5 seconds delay

    function process() {
        // 1. Get the current author from the Header
        const userLinks = Array.from(document.querySelectorAll('.Header .Warnings .row a.WikiLink'));
        const currentAuthor = userLinks.length > 0 ? userLinks[0].innerText.trim() : null;

        if (!currentAuthor) {
            console.log("Waiting for article to load...");
            setTimeout(process, 1000);
            return;
        }

        // 2. Check if the author is in our target list
        const isMatch = TARGET_USERS.some(user => currentAuthor.includes(user));

        // 3. Select controls
        const evalDiv = document.querySelector('.Evaluation');
        const yesButton = document.querySelector('button[title="accepted"]');
        const saveButton = document.querySelector('button.constructive');
        const skipButton = document.querySelector('button.WikiButton:not(.constructive)');

        if (isMatch) {
            console.log(`%c ✅ Match found: ${currentAuthor}. Accepting...`, "color: #2e7d32; font-weight: bold;");
            
            if (yesButton && !yesButton.classList.contains('active')) {
                yesButton.click();
            }

            // Wait for React to enable the Save button
            setTimeout(() => {
                if (saveButton && !saveButton.disabled) {
                    saveButton.click();
                    console.log("Saved. Moving to next...");
                    setTimeout(process, ACTION_DELAY);
                } else {
                    console.warn("Save button disabled. Selection might have failed.");
                }
            }, 800);

        } else {
            console.log(`⏭️ Skipping: ${currentAuthor} is not in the list.`);
            
            if (skipButton) {
                skipButton.click();
                setTimeout(process, ACTION_DELAY);
            } else {
                console.error("Skip button not found.");
            }
        }
    }

    process();
})();
