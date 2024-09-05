from flask import Flask, render_template, request, redirect, url_for
from werkzeug.utils import secure_filename
import os
import cv2
import numpy as np

app = Flask(__name__)

# Define the upload folder and allowed extensions
UPLOAD_FOLDER = 'uploads'
ALLOWED_EXTENSIONS = {'png', 'jpg', 'jpeg', 'gif'}
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER


# Function to check if the file extension is allowed
def allowed_file(filename):
    return '.' in filename and filename.rsplit('.', 1)[1].lower() in ALLOWED_EXTENSIONS


# Function to perform image augmentation
def augment_image(image):
    # Example augmentation: Flip horizontally
    augmented_image = cv2.flip(image, 1)
    return augmented_image


@app.route('/')
def index():
    return render_template('index.html')


@app.route('/upload', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        # Check if the post request has the file part
        if 'file' not in request.files:
            return redirect(request.url)

        file = request.files['file']

        # If user does not select file, browser also submit an empty part without filename
        if file.filename == '':
            return redirect(request.url)

        if file and allowed_file(file.filename):
            # Save the uploaded file to the uploads folder
            filename = secure_filename(file.filename)
            filepath = os.path.join(app.config['UPLOAD_FOLDER'], filename)
            file.save(filepath)

            # Read the uploaded image
            image = cv2.imread(filepath)

            # Perform augmentation
            augmented_image = augment_image(image)

            # Save the augmented image
            augmented_filename = 'augmented_' + filename
            augmented_filepath = os.path.join(app.config["UPLOAD_FOLDER"], augmented_filename)
            cv2.imwrite(augmented_filepath, augmented_image)

            # Return the filenames to display the original and augmented images
            return render_template('result.html', original=filename, augmented=augmented_filename)

    return redirect(url_for('index'))


if __name__ == '__main__':
    app.run(debug=True)
