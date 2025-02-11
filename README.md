<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MSU Research Terminal</title>
    <style>
        body { 
            background-color: black; 
            color: #00FF00; 
            font-family: "Courier New", Courier, monospace; 
            padding: 20px; 
        }
        #terminal, #library-terminal { 
            white-space: pre-wrap; 
            display: none; 
        }
        #input, #passkey-input { 
            background: black; 
            color: #00FF00; 
            border: none; 
            font-family: "Courier New", Courier, monospace; 
            width: 100%; 
            margin-top: 10px; 
        }
        #boot-screen { 
            position: fixed; 
            top: 0; 
            left: 0; 
            width: 100%; 
            height: 100%; 
            background: black; 
            color: #00FF00; 
            font-family: "Courier New", Courier, monospace; 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            flex-direction: column; 
        }
    </style>
</head>
<body>
    <div id="boot-screen">BOOTING SYSTEM...<br>Please Wait...</div>
    <div id="terminal"></div>
    <div id="library-terminal">Welcome to the MSU Library Archives. Type the name of a book to retrieve its passage or enter the restricted access code:</div>
    <input type="text" id="input" autofocus placeholder="Enter username..." style="display:none;">
    <input type="text" id="passkey-input" placeholder="Enter restricted access code..." style="display:none;">

    <audio id="keypress-sound" src="data:audio/wav;base64,UklGRiQAAABXQVZFZm10IBAAAAABAAEAIlYAAESsAAACABAAZGF0YRAAAAAA"></audio>
    <audio id="boot-sound" src="data:audio/wav;base64,UklGRlIAAABXQVZFZm10IBAAAAABAAEAIlYAAESsAAACABAAZGF0YVQAAAAA"></audio>
    <audio id="success-sound" src="data:audio/wav;base64,UklGRiQAAABXQVZFZm10IBAAAAABAAEAIlYAAESsAAACABAAZGF0YW8AAAAA"></audio>
    <audio id="error-sound" src="data:audio/wav;base64,UklGRiQAAABXQVZFZm10IBAAAAABAAEAIlYAAESsAAACABAAZGF0YW8AAAAA"></audio>

    <script>
        const books = {
            "Tale of Two Cities": "It was the best of times, it was the worst of times, it was the age of wisdom...",
            "Moby Dick": "Call me Ishmael. Some years ago—never mind how long precisely—having little or no money in my purse...",
            "War and Peace": "Well, Prince, so Genoa and Lucca are now just family estates of the Buonapartes...",
            "1984": "It was a bright cold day in April, and the clocks were striking thirteen...",
            "Fahrenheit 451": "It was a pleasure to burn. It was a special pleasure to see things eaten, to see things blackened and changed...",
            "Pride and Prejudice": "It is a truth universally acknowledged, that a single man in possession of a good fortune, must be in want of a wife...",
            "Great Expectations": "My father’s family name being Pirrip, and my Christian name Philip, my infant tongue could make of both names nothing longer or more explicit than Pip...",
            "The Odyssey": "Tell me, O Muse, of that ingenious hero who travelled far and wide after he had sacked the famous town of Troy...",
            "The Iliad": "Sing, O goddess, the anger of Achilles son of Peleus, that brought countless ills upon the Achaeans...",
            "Jane Eyre": "There was no possibility of taking a walk that day. We had been wandering, indeed, in the leafless shrubbery an hour in the morning...",
            "Frankenstein": "You will rejoice to hear that no disaster has accompanied the commencement of an enterprise which you have regarded with such evil forebodings...",
            "Dracula": "3 May. Bistritz.— Left Munich at 8:35 P.M., on 1st May, arriving at Vienna early next morning...",
            "Les Misérables": "When he reached the last house in the village, he halted to glance back...",
            "Wuthering Heights": "1801.—I have just returned from a visit to my landlord—the solitary neighbour that I shall be troubled with...",
            "The Great Gatsby": "In my younger and more vulnerable years my father gave me some advice that I’ve been turning over in my mind ever since...",
            "Crime and Punishment": "On an exceptionally hot evening early in July a young man came out of the garret in which he lodged in S. Place...",
            "Anna Karenina": "Happy families are all alike; every unhappy family is unhappy in its own way...",
            "The Catcher in the Rye": "If you really want to hear about it, the first thing you’ll probably want to know is where I was born...",
            "To Kill a Mockingbird": "When he was nearly thirteen, my brother Jem got his arm badly broken at the elbow..."
        };

        let stage = 0;
        let username = "";
        let password = "";

        function playSound(id) {
            document.getElementById(id).play();
        }

        setTimeout(() => {
            playSound("boot-sound");
            document.getElementById("boot-screen").style.display = "none";
            document.getElementById("terminal").style.display = "block";
            document.getElementById("input").style.display = "block";
            document.getElementById("passkey-input").style.display = "block";
            document.getElementById("terminal").innerText = "Enter username:";
        }, 3000);

        document.getElementById("input").addEventListener("keypress", function(event) {
            playSound("keypress-sound");
            if (event.key === "Enter") {
                let userInput = this.value.trim();
                this.value = "";

                if (stage === 0) {
                    if (userInput === "Halloway") {
                        username = userInput;
                        stage++;
                        playSound("success-sound");
                        document.getElementById("terminal").innerText += "\nUsername accepted. Enter password:";
                    } else {
                        playSound("error-sound");
                        document.getElementById("terminal").innerText += "\nACCESS DENIED. Try again.";
                    }
                } else if (stage === 1) {
                    if (userInput === "EvilArch1987") {
                        password = userInput;
                        stage++;
                        playSound("success-sound");
                        document.getElementById("terminal").innerText += "\nACCESS GRANTED.\n";
                        setTimeout(() => {
                            document.getElementById("terminal").style.display = "none";
                            document.getElementById("library-terminal").style.display = "block";
                        }, 2000);
                    } else {
                        playSound("error-sound");
                        document.getElementById("terminal").innerText += "\nACCESS DENIED. Try again.";
                    }
                } else if (stage === 2) {
                    if (books[userInput]) {
                        playSound("success-sound");
                        document.getElementById("library-terminal").innerText += `\nRetrieving passage from '${userInput}':\n${books[userInput]}`;
                    } else {
                        playSound("error-sound");
                        document.getElementById("library-terminal").innerText += "\nBook not found. Try another title.";
                    }
                }
            }
        });

        document.getElementById("passkey-input").addEventListener("keypress", function(event) {
            playSound("keypress-sound");
            if (event.key === "Enter") {
                let passkeyInput = this.value.trim();
                this.value = "";

                if (passkeyInput === "Woods") {
                    playSound("success-sound");
                    document.getElementById("library-terminal").innerText += "\nPasskey accepted. Coordinates: 42.71990972470436, -84.47323065544654";
                } else {
                    playSound("error-sound");
