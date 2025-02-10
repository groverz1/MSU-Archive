<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MSU Research Terminal</title>
    <style>
        body { background-color: black; color: green; font-family: monospace; padding: 20px; }
        #terminal, #library-terminal, #passkey-terminal { white-space: pre-wrap; display: none; }
        #input, #passkey-input { background: black; color: green; border: none; font-family: monospace; width: 100%; }
        #boot-screen { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: black; color: green; font-family: monospace; display: flex; align-items: center; justify-content: center; flex-direction: column; }
    </style>
</head>
<body>
    <div id="boot-screen">BOOTING SYSTEM...<br>Please Wait...</div>
    <div id="terminal"></div>
    <div id="library-terminal">Welcome to the MSU Library Archives. Type the name of a book to retrieve its passage:</div>
    <input type="text" id="input" autofocus placeholder="Enter username..." style="display:none;">
    <input type="text" id="passkey-input" placeholder="Enter passkey..." style="display:none; margin-top: 10px;">
    <div id="passkey-terminal">Enter the passkey to reveal the coordinates:</div>

    <script>
        const books = {
            "Tale of Two Cities": "It was the best of times, it was the worst of times, it was the age of wisdom...",
            "Moby Dick": "Call me Ishmael. Some years ago—never mind how long precisely—having little or no money in my purse...",
            "War and Peace": "Well, Prince, so Genoa and Lucca are now just family estates of the Buonapartes...",
            "1984": "It was a bright cold day in April, and the clocks were striking thirteen...",
            "Fahrenheit 451": "It was a pleasure to burn. It was a special pleasure to see things eaten, to see things blackened and changed..."
        };

        let stage = 0;
        let username = "";
        let password = "";

        setTimeout(() => {
            document.getElementById("boot-screen").style.display = "none";
            document.getElementById("terminal").style.display = "block";
            document.getElementById("input").style.display = "block";
            document.getElementById("terminal").innerText = "Enter username:";
        }, 3000);

        document.getElementById("input").addEventListener("keypress", function(event) {
            if (event.key === "Enter") {
                let userInput = this.value.trim();
                this.value = "";

                if (stage === 0) {
                    if (userInput === "Halloway") {
                        username = userInput;
                        stage++;
                        document.getElementById("terminal").innerText += "\nUsername accepted. Enter password:";
                    } else {
                        document.getElementById("terminal").innerText += "\nACCESS DENIED. Try again.";
                    }
                } else if (stage === 1) {
                    if (userInput === "EvilArch1987") {
                        password = userInput;
                        stage++;
                        document.getElementById("terminal").innerText += "\nACCESS GRANTED.\n";
                        setTimeout(() => {
                            document.getElementById("terminal").style.display = "none";
                            document.getElementById("library-terminal").style.display = "block";
                            document.getElementById("passkey-input").style.display = "block";
                        }, 2000);
                    } else {
                        document.getElementById("terminal").innerText += "\nACCESS DENIED. Try again.";
                    }
                } else if (stage === 2) {
                    if (books[userInput]) {
                        document.getElementById("library-terminal").innerText += `\nRetrieving passage from '${userInput}':\n${books[userInput]}`;
                    } else {
                        document.getElementById("library-terminal").innerText += "\nBook not found. Try another title.";
                    }
                }
            }
        });

        document.getElementById("passkey-input").addEventListener("keypress", function(event) {
            if (event.key === "Enter") {
                let passkeyInput = this.value.trim();
                this.value = "";

                if (passkeyInput === "Woods") {
                    document.getElementById("library-terminal").innerText += "\nPasskey accepted. Coordinates: 42.71990972470436, -84.47323065544654";
                } else {
                    document.getElementById("library-terminal").innerText += "\nIncorrect passkey. Try again.";
                }
            }
        });
    </script>
</body>
</html>
