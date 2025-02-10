# MSU-Archive<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MSU Tome Hunt - Terminal</title>
    <style>
        body { background-color: black; color: green; font-family: monospace; padding: 20px; }
        #terminal { white-space: pre-wrap; }
        #input { background: black; color: green; border: none; font-family: monospace; width: 100%; }
    </style>
</head>
<body>
    <div id="terminal"></div>
    <input type="text" id="input" autofocus placeholder="Enter the passkey...">
    
    <script>
        const passages = [
            "1. It was the best of times, it was the worst of times, it was the age of wisdom, it was the age of foolishness.",
            "2. Call me Ishmael. Some years ago—never mind how long precisely—having little or no money in my purse.",
            "3. All happy families are alike; each unhappy family is unhappy in its own way.",
            "4. It is a truth universally acknowledged, that a single man in possession of a good fortune, must be in want of a wife.",
            "5. The sky above the port was the color of television, tuned to a dead channel.",
            "6. It was a bright cold day in April, and the clocks were striking thirteen.",
            "7. You are about to begin reading Italo Calvino’s If on a winter’s night a traveler.",
            "8. It was a pleasure to burn. It was a special pleasure to see things eaten, to see things blackened and changed.",
            "9. The man in black fled across the desert, and the gunslinger followed.",
            "10. Mr. and Mrs. Dursley, of number four, Privet Drive, were proud to say that they were perfectly normal, thank you very much."
        ];
        
        const ottendorfCipher = "3,5,2 | 6,4,1 | 8,3,3"; // Example cipher: (Book, Line, Word)
        const correctPasskey = "families clocks burn"; // Extracted words in order
        
        document.getElementById("terminal").innerText = "BOOTING...\nWelcome to the MSU Research Archive\n\nDisplaying documents:\n" + passages.join("\n\n") + "\n\nOttendorf Cipher: " + ottendorfCipher;
        
        document.getElementById("input").addEventListener("keypress", function(event) {
            if (event.key === "Enter") {
                let userInput = this.value.trim().toLowerCase();
                this.value = "";
                if (userInput === correctPasskey) {
                    document.getElementById("terminal").innerText += "\nACCESS GRANTED!\nNext clue location unlocked!";
                } else {
                    document.getElementById("terminal").innerText += "\nACCESS DENIED. Try again.";
                }
            }
        });
    </script>
</body>
</html>
