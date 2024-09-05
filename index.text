<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Image Augmentation</title>
    <style>
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
    </style>
</head>
<body>
    <div id="container">
        <h1>Image Augmentation</h1>
        <input type="file" id="fileInput" accept="image/*" required>
        <button onclick="applyOperation('rotate')">Rotate</button>
        <button onclick="applyOperation('grayscale')">Grayscale</button>
        <button onclick="applyOperation('blur')">Blur</button>
        <button onclick="applyOperation('brightness')">Adjust Brightness</button>
        <button onclick="applyOperation('contrast')">Adjust Contrast</button>
        <button onclick="applySize('passport')">Passport Size</button>
        <button onclick="applySize('stamp')">Stamp Size</button>
        <button onclick="applyOperations(['rotate', 'grayscale'])">Rotate + Grayscale</button>
        <button onclick="applyOperations(['blur', 'brightness'])">Blur + Brightness</button>
        <!-- Add more buttons for other augmentation operations -->
        <button onclick="redirectToRecognition()">Upload Image</button>
        <div id="imageContainer"></div>

    </div>

    <script>
        function applyOperation(operation) {
            var fileInput = document.getElementById('fileInput');
            var file = fileInput.files[0];

            if (file) {
                var reader = new FileReader();
                reader.onload = function(e) {
                    var img = new Image();
                    img.src = e.target.result;
                    img.onload = function() {
                        var augmentedImage = img;
                        augmentedImage = applyOperationToImage(augmentedImage, operation);
                        displayAugmentedImage(augmentedImage);
                    };
                };
                reader.readAsDataURL(file);
            }
        }

        function applySize(size) {
            var imageContainer = document.getElementById('imageContainer');
            var image = imageContainer.querySelector('img');

            if (image) {
                var canvas = document.createElement('canvas');
                var ctx = canvas.getContext('2d');

                var maxWidth, maxHeight;
                if (size === 'passport') {
                    maxWidth = 150;
                    maxHeight = 200;
                } else if (size === 'stamp') {
                    maxWidth = 100;
                    maxHeight = 100;
                }

                var widthRatio = maxWidth / image.width;
                var heightRatio = maxHeight / image.height;
                var ratio = Math.min(widthRatio, heightRatio);

                canvas.width = image.width * ratio;
                canvas.height = image.height * ratio;

                ctx.drawImage(image, 0, 0, canvas.width, canvas.height);

                var resizedImage = new Image();
                resizedImage.src = canvas.toDataURL();
                displayAugmentedImage(resizedImage);
            }
        }

        function applyOperations(operations) {
            var fileInput = document.getElementById('fileInput');
            var file = fileInput.files[0];

            if (file) {
                var reader = new FileReader();
                reader.onload = function(e) {
                    var img = new Image();
                    img.src = e.target.result;
                    img.onload = function() {
                        var augmentedImage = img;
                        operations.forEach(function(operation) {
                            augmentedImage = applyOperationToImage(augmentedImage, operation);
                        });
                        displayAugmentedImage(augmentedImage);
                    };
                };
                reader.readAsDataURL(file);
            }
        }

        function applyOperationToImage(image, operation) {
            var canvas = document.createElement('canvas');
            var ctx = canvas.getContext('2d');
            canvas.width = image.width;
            canvas.height = image.height;

            switch (operation) {
                case 'rotate':
                    ctx.translate(canvas.width / 2, canvas.height / 2);
                    ctx.rotate(Math.PI / 4); // Rotate by 45 degrees
                    ctx.drawImage(image, -image.width / 2, -image.height / 2);
                    break;
                case 'grayscale':
                    ctx.drawImage(image, 0, 0);
                    var imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
                    var data = imageData.data;
                    for (var i = 0; i < data.length; i += 4) {
                        var avg = (data[i] + data[i + 1] + data[i + 2]) / 3;
                        data[i] = avg; // Red
                        data[i + 1] = avg; // Green
                        data[i + 2] = avg; // Blue
                    }
                    ctx.putImageData(imageData, 0, 0);
                    break;
                case 'blur':
                    ctx.filter = 'blur(5px)';
                    ctx.drawImage(image, 0, 0);
                    break;
                case 'brightness':
                    ctx.filter = 'brightness(1.5)';
                    ctx.drawImage(image, 0, 0);
                    break;
                case 'contrast':
                    ctx.filter = 'contrast(150%)';
                    ctx.drawImage(image, 0, 0);
                    break;
                // Add cases for other augmentation operations
            }

            var augmentedImage = new Image();
            augmentedImage.src = canvas.toDataURL();
            return augmentedImage;
        }

        function displayAugmentedImage(augmentedImage) {
            var imageContainer = document.getElementById('imageContainer');
            imageContainer.innerHTML = '';
            imageContainer.appendChild(augmentedImage);
        }
        function redirectToUpload() {
            window.location.href = "Recognition.html";
        }

    </script>
</body>
</html>
