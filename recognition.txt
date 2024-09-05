<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Image Augmentation with Recognition</title>
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@3.12.0"></script>
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/mobilenet@2.0.4"></script>
    <style>
        /* CSS styles */
        body {
            font-family: Arial, sans-serif;
            background-color: #f8f8f8;
            margin: 0;
            padding: 0;
        }
        #container {
            max-width: 600px;
            margin: 50px auto;
            background-color: #fff;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        h1 {
            text-align: center;
            color: #333;
        }
        #fileInput {
            display: block;
            margin: 0 auto 20px;
        }
        button {
            display: block;
            width: 100%;
            max-width: 200px;
            margin: 0 auto 10px;
            padding: 15px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            background-color: #4CAF50;
            color: white;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #45a049;
        }
        #imageContainer {
            text-align: center;
            margin-top: 30px;
        }
        #imageContainer img {
            max-width: 100%;
            height: auto;
            border-radius: 5px;
            box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
        }
        #resultContainer {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div id="container">
        <h1>Image Augmentation with Recognition</h1>
        <input type="file" id="fileInput" accept="image/*" required>
        <button onclick="applyRecognition()">Recognize Image</button>
        <div id="imageContainer"></div>
        <div id="resultContainer"></div>
    </div>

    <script>
        // Load MobileNet model for image classification
        let model;
        async function loadModel() {
            model = await mobilenet.load();
        }
        loadModel();

        async function applyRecognition() {
            const fileInput = document.getElementById('fileInput');
            const file = fileInput.files[0];

            if (file) {
                const reader = new FileReader();
                reader.onload = async function(e) {
                    const img = new Image();
                    img.src = e.target.result;
                    img.onload = async function() {
                        // Display the original image
                        displayImage(img);
                        // Classify the image
                        const predictions = await classifyImage(img);
                        // Display the classification results
                        displayResults(predictions);
                    };
                };
                reader.readAsDataURL(file);
            }
        }

        async function classifyImage(img) {
            const predictions = await model.classify(img);
            return predictions;
        }

        function displayImage(img) {
            const imageContainer = document.getElementById('imageContainer');
            imageContainer.innerHTML = '';
            imageContainer.appendChild(img);
        }

        function displayResults(predictions) {
            const resultContainer = document.getElementById('resultContainer');
            resultContainer.innerHTML = '<h2>Classification Results:</h2>';
            const ul = document.createElement('ul');
            predictions.forEach(prediction => {
                const li = document.createElement('li');
                li.textContent = `${prediction.className}: ${Math.round(prediction.probability * 100)}%`;
                ul.appendChild(li);
            });
            resultContainer.appendChild(ul);
        }
    </script>
</body>
</html>
