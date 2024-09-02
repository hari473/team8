const startBtn = document.getElementById('start-btn');
const recognizedTextElement = document.getElementById('recognized-text');
const translatedTextElement = document.getElementById('translated-text');
const statusElement = document.getElementById('status');
const languageSelect = document.getElementById('language');

let recognition;

if ('webkitSpeechRecognition' in window) {
    recognition = new webkitSpeechRecognition();
    recognition.continuous = false;
    recognition.interimResults = false;
    recognition.lang = 'en-US';

    recognition.onstart = () => {
        statusElement.textContent = "Listening...";
    };

    recognition.onresult = (event) => {
        const transcript = event.results[0][0].transcript;
        recognizedTextElement.textContent = transcript;

        // Translate the recognized text
        translateText(transcript);
    };

    recognition.onerror = (event) => {
        statusElement.textContent = 'Error occurred in recognition: ' + event.error;
    };

    recognition.onend = () => {
        statusElement.textContent = "Recognition stopped.";
    };
} else {
    statusElement.textContent = "Speech Recognition API not supported in this browser.";
}

startBtn.addEventListener('click', () => {
    recognition.lang = languageSelect.value;
    recognition.start();
});

function translateText(text) {
    const targetLanguage = languageSelect.value.split('-')[0]; // Get the language code
   const url = `https://api.mymemory.translated.net/get?q=${encodeURIComponent(text)}&langpair=en|${targetLanguage}`;


    fetch(url)
        .then(response => response.json())
        .then(data => {
            if (data.responseData.translatedText) {
                translatedTextElement.textContent = data.responseData.translatedText;
            } else {
                translatedTextElement.textContent = "Translation error.";
            }
        })
        .catch(error => {
            translatedTextElement.textContent = "Error occurred while translating.";
        });
}