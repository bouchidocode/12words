<!DOCTYPE html>
<html>
  <head>
    <script src="https://apis.google.com/js/api.js"></script>
    <style>
      .selected-words {
        background-color: lightgray;
        padding: 5px;
        margin-top: 10px;
        width: 100%;
        height: 50px;
      }
    </style>
  </head>
  <body>
    <button id="generate-btn">Generate Random Words</button>
    <div class="selected-words" id="selected-words"></div>
    <script>
      const generateBtn = document.getElementById("generate-btn");
      const selectedWordsDiv = document.getElementById("selected-words");
      let words;

      gapi.load("client", function() {
        gapi.client
          .init({
            apiKey: "AIzaSyDkmhrf20KDeb9n0q6-6Yg7FqpnfOZg4wo",
            discoveryDocs: [
              "https://sheets.googleapis.com/$discovery/rest?version=v4"
            ]
          })
          .then(function() {
            generateBtn.addEventListener("click", function() {
              gapi.client.sheets.spreadsheets.values
                .get({
                  spreadsheetId: "1ZsnHfdxOCGpRZvkOErZtjOkYT4MmF8hmiGME1NjNtCc",
                  range: "Sheet1!A1:A2048"
                })
                .then(function(response) {
                  words = response.result.values;
                  let selectedWords = [];
                  for (let i = 0; i < 12; i++) {
                    let randomIndex = Math.floor(Math.random() * words.length);
                    let randomWord = words[randomIndex];
                    if (!selectedWords.includes(randomWord)) {
                      selectedWords.push(randomWord);
                    } else {
                      i--;
                    }
                  }
                  selectedWordsDiv.innerText = selectedWords.join(" ");
                  navigator.clipboard.writeText(selectedWords.join(" ")).then(
                    function() {
                      console.log("Copied to clipboard");
                    },
                    function(err) {
                      console.error("Could not copy to clipboard: ", err);
                    }
                  );
                });
            });
          });
      });
    </script>
  </body>
</html>
