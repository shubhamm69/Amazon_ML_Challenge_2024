# ML Challenge Problem Statement

## Feature Extraction from Images

In this challenge, the goal is to create a machine learning model that extracts entity values from images. This capability is crucial in industries such as healthcare, e-commerce, and content moderation, where extracting product information is vital. As online marketplaces grow, many products lack textual descriptions, so extracting key details directly from images becomes essential. Information such as weight, volume, voltage, wattage, dimensions, and more is critical for digital stores.

### Data Description: 

The dataset consists of the following columns: 

1. **index:** A unique identifier (ID) for the data sample.
2. **image_link:** Public URL where the product image is available for download. Example link - `https://m.media-amazon.com/images/I/71XfHPR36-L.jpg`. 
    - To download images, use the `download_images` function from `utils.py`. See sample code in `test.ipynb`.
3. **group_id:** Category code of the product.
4. **entity_name:** Product entity name (e.g., “item_weight”).
5. **entity_value:** Product entity value (e.g., “34 gram”).
    - **Note:** For `test.csv`, you will not see the column `entity_value` as it is the target variable.

### Process Flow:

1. **Download the Test and Train Datasets**:
   - First, download the train and test images using the `download_images` function available in `utils.py`.
   - You can find example usage in `test.ipynb`. This notebook walks you through downloading the images and saving them locally.

2. **Extract Text from Images**:
   - After downloading the train and test images, run the `TrainData_OCR.ipynb` and `TestData_OCR.ipynb` notebooks. These notebooks will extract the text from each image and store the information in `train_ocr.csv` and `test_ocr.csv`, respectively.
   - These CSV files contain the text extracted from the images and will serve as the input for training and testing the BERT QA model.

3. **Train the BERT QA Model**:
   - Use the `train_ocr.csv` file to train the BERT model. The goal is to map the extracted text to the correct entity values.
   - Once trained, use the model to make predictions on the `test_ocr.csv` file. This will generate a file with predicted values for each entity based on the extracted text.
   - The predictions should be saved in a format matching the `test_out.csv`.

4. **Test and Evaluate**:
   - The `test.csv` file (in the `dataset` folder) contains the test data without the target values. Use this file in conjunction with the `test_ocr.csv` to map the results from each image and save the results in the `test_out.csv` file.

5. **Sanity Check**:
   - Run the `sanity.py` script to check for any missing rows or columns and ensure the predictions are in the correct format. This script will validate your output to ensure that it complies with the format required for submission.
   - Make sure to check for proper formatting, such as index alignment, entity units, and valid predictions.

### Output Format:

The final output should be a CSV with 2 columns:

1. **index:** The unique identifier (ID) of the data sample. This index should match the `test.csv` index.
2. **prediction:** A string with the format: “x unit” where `x` is a float number and `unit` is one of the allowed units (see the appendix for valid units). Examples include “2 gram”, “12.5 centimetre”, “2.56 ounce”.
    - If no value is found in the image, return an empty string `“”`. 
    - **Note:** Make sure the number of output samples matches the number of test records; otherwise, the file will not be evaluated.

### Steps to Execute:

1. **Download Images**: Follow the instructions in `test.ipynb` to download the images using `utils.py`.
2. **Extract Text**: Run `TrainData_OCR.ipynb` and `TestData_OCR.ipynb` to extract text and generate `train_ocr.csv` and `test_ocr.csv`.
3. **Train the Model**: Use the extracted text from `train_ocr.csv` to train the BERT QA model. Save the predictions for test data into `test_out.csv`.
4. **Run Sanity Check**: Execute the `sanity.py` script to validate your predictions and ensure they match the required format.

### Constraints

1. **Sanity Checker**: Format your output file to match `sample_test_out.csv`. Use the `sanity.py` script to verify your output. The script ensures that the file format is correct. A message like `Parsing successful for file: ...csv` indicates correct formatting.
2. **Allowed Units**: Use only the units listed in `constants.py` or the appendix. Predictions using other units will be marked as invalid during evaluation.

### Evaluation Criteria

The submissions will be evaluated based on the F1 score, a standard metric for classification and extraction tasks. The F1 score balances precision and recall:

- **Precision** = True Positives / (True Positives + False Positives)
- **Recall** = True Positives / (True Positives + False Negatives)
  
The evaluation will classify predictions into:
1. **True Positives**: If the prediction is correct.
2. **False Positives**: If the prediction is incorrect.
3. **False Negatives**: If no prediction was made but there was a ground truth.
4. **True Negatives**: If no prediction was needed and none was made.

### Output File

- `test_out.csv` with predictions in the exact format required.

### Appendix

Allowed units are:

```
entity_unit_map = {
  "width": {
    "centimetre", "foot", "millimetre", "metre", "inch", "yard"
  },
  "depth": {
    "centimetre", "foot", "millimetre", "metre", "inch", "yard"
  },
  "height": {
    "centimetre", "foot", "millimetre", "metre", "inch", "yard"
  },
  "item_weight": {
    "milligram", "kilogram", "microgram", "gram", "ounce", "ton", "pound"
  },
  "maximum_weight_recommendation": {
    "milligram", "kilogram", "microgram", "gram", "ounce", "ton", "pound"
  },
  "voltage": {
    "millivolt", "kilovolt", "volt"
  },
  "wattage": {
    "kilowatt", "watt"
  },
  "item_volume": {
    "cubic foot", "microlitre", "cup", "fluid ounce", "centilitre", "imperial gallon", "pint", "decilitre", "litre", "millilitre", "quart", "cubic inch", "gallon"
  }
}