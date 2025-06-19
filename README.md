# AI-Powered PDF to Translated LaTeX Book Converter

## 1. Project Goal

The primary goal of this project is to take a PDF file composed of scanned images (an image-based PDF) and convert it into a fully translated, compilable LaTeX book.

The script uses Google's Gemini Vision model to perform Optical Character Recognition (OCR) on each page, translate the extracted text into a specified language, and format the result into a `.tex` file, ready for further editing and compilation.

## 2. How It Works

This project leverages a Python script to automate a multi-step process:

1.  **PDF Parsing**: It opens a PDF file and iterates through it page by page using the `PyMuPDF` library.
2.  **Image Rendering**: Each page is rendered as a high-resolution image to prepare it for OCR.
3.  **AI-Powered OCR & Translation**: The page image is sent to the **Google Gemini Pro Vision API**. A carefully crafted prompt instructs the model to perform OCR on the image, translate the text to the target language, and format the output directly into LaTeX code.
4.  **Incremental Saving**: To prevent data loss on large documents, the script saves the processed content to the output `.tex` file after each page.
5.  **Final Output**: The result is a single `output.tex` file that can be compiled using any standard LaTeX distribution (like TeX Live, MiKTeX, or online services like Overleaf) to produce a final, translated PDF.

## 3. How to Use

Follow these steps to set up and run the project.

### Step 1: Prerequisites

First, ensure you have the required tools and libraries.

-   **Python 3.7+** installed.
-   A **Google AI Studio API Key**. You can get one for free from the [Google AI Studio](https://aistudio.google.com/).
-   Install the necessary Python libraries by running the following command in your terminal:
    ```bash
    pip install google-generativeai pymupdf python-dotenv Pillow
    ```

### Step 2: Set Up Your API Key

For security, the script loads your API key from a `.env` file.

1.  In the root of your project folder, create a new file named `.env`.
2.  Add the following line to the file, replacing `"YOUR_API_KEY_HERE"` with your actual key:
    ```
    GOOGLE_API_KEY="YOUR_API_KEY_HERE"
    ```
    **Important:** Never share this file or commit it to a public repository like GitHub.

### Step 3: Configure the Script

Open the main Python script (`test.py` or your chosen name) and edit the variables in the **Configuration** section to match your needs.

#### Configuration Variables

-   `OUTPUT_LANGUAGE`: The target language for the translation.
    -   **Example**: `"Portuguese (Brazil)"`, `"Japanese"`, `"French"`, `"English"`
-   `PDF_PATH`: The full path to the input PDF file you want to process.
    -   **Example**: `"C:/Users/leona/Downloads/MyBook.pdf"` or `"./input/MyBook.pdf"`
-   `LATEX_OUTPUT_PATH`: The name of the file where the LaTeX output will be saved.
    -   **Example**: `"output.tex"`
-   `PAGES_TO_PROCESS`: Controls how many pages of the PDF are processed. This is very useful for testing.
    -   **To test the first 5 pages**: Set it to `5`.
    -   **To process the entire book**: Set it to a very high number, like `10000`. The script will automatically stop at the last page of the document.

### Step 4: Run the Script

Execute the script from your terminal:

```bash
python test.py
```

You will see the progress printed in the terminal as it processes each page. The `output.tex` file will be updated incrementally.

### Step 5: Compile the LaTeX File

Once the script is finished, you will have a complete `output.tex` file. Use a LaTeX compiler to generate the final PDF. If you have a local LaTeX distribution, you might run a command like:

```bash
pdflatex output.tex
```

You may need to run it twice for the table of contents and references to be correctly generated.

## 4. Notes and Limitations

-   **API Costs**: Processing a large number of pages will consume your Google AI API quota and may incur costs depending on your usage. Use the `PAGES_TO_PROCESS` variable to test on a small sample first.
-   **OCR/Translation Accuracy**: While the AI is very powerful, the quality of the OCR and translation depends on the source image quality. The final LaTeX output may require manual proofreading and corrections.
-   **Errors**: If the AI fails to process a page, the script will insert a comment like `%% Error processing this page... %%` into the `.tex` file and continue to the next page, ensuring your progress is not lost.