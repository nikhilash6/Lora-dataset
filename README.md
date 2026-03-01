# LoRA Dataset Architect

A comprehensive tool for cropping, curating, and captioning image datasets for LoRA training.

## Running the Application

You can run this application in two modes:
1. **Cloud Mode (Gemini API)**: The easiest way. Requires a free Google Gemini API key. No local GPU required.
2. **Local Mode (Python API)**: Runs completely offline using open-source models like Qwen2-VL or Joy Caption. Requires a powerful local GPU (Nvidia RTX 3060 or better with at least 8GB VRAM).

---

### Method 1: Cloud Mode (Easiest)

If you don't have a powerful GPU or don't want to deal with Python environments, you can use the Gemini API.

1. Go to [Google AI Studio](https://aistudio.google.com/app/apikey) and create a free API key.
2. Open the app, click the **Settings** icon (top right).
3. Select **Gemini API (Cloud)** and paste your API key.
4. The app will now use Gemini 2.5 Flash to caption your images automatically.

---

### Method 2: Local Mode (Advanced / Offline)

If you want to run the captioning entirely locally using models like **Qwen2.5-VL** or **Joy Caption**, you need to set up a local Python server. The React app will send your images to this local server instead of Google.

#### Prerequisites

1. **Nvidia GPU**: You need an Nvidia GPU with at least 8GB of VRAM (12GB+ recommended).
2. **Python 3.10 or 3.11**: Download and install from [python.org](https://www.python.org/downloads/).
   - *Important:* During installation, make sure to check the box that says **"Add Python to PATH"**.
3. **Git**: Download and install from [git-scm.com](https://git-scm.com/downloads).

#### Step 1: Install PyTorch (with CUDA)

Open your terminal (Command Prompt or PowerShell on Windows) and install PyTorch. You need the version that supports your GPU (CUDA).

```bash
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

#### Step 2: Install Required Python Packages

Next, install the libraries required to run the local API server and load the AI models:

```bash
pip install fastapi uvicorn transformers pillow accelerate qwen-vl-utils
```

#### Step 3: Download the Model (Automatic)

You don't need to manually download the model files from a website. The `transformers` library will automatically download the required files the first time you run the script and cache them on your hard drive (usually in `C:\Users\YourName\.cache\huggingface`).

The provided `local_caption_server.py` script uses `Qwen/Qwen2-VL-7B-Instruct` by default, which is an extremely powerful open-source vision model.

*Note: The initial download is about 15GB, so it may take a while depending on your internet speed.*

#### Step 4: Run the Local Server

In your terminal, navigate to the folder where you saved this project, and run the Python script:

```bash
python local_caption_server.py
```

You should see output indicating that the model is downloading/loading. Once it says `Application startup complete`, the server is running on `http://localhost:8000`.

#### Step 5: Connect the App to your Local Server

1. Open the React app in your browser.
2. Click the **Settings** icon (top right).
3. Select **Local Python API**.
4. Ensure the **Local API URL** is set to `http://localhost:8000`.
5. Close the settings and start captioning! The app will now send images to your local Python script.

---

### Troubleshooting Local Mode

- **"CUDA out of memory"**: Your GPU doesn't have enough VRAM to load the 7B parameter model. You can try loading a smaller model (like a 2B parameter model) by changing the `MODEL_ID` in `local_caption_server.py` to `"Qwen/Qwen2-VL-2B-Instruct"`.
- **"Connection Refused" / "Local API failed"**: Ensure the Python script is actually running in your terminal and hasn't crashed. Ensure the URL in the app settings exactly matches the URL printed in the terminal (usually `http://0.0.0.0:8000` or `http://localhost:8000`).
- **Missing modules**: If Python complains about missing modules (e.g., `ModuleNotFoundError: No module named 'fastapi'`), ensure you ran the `pip install` commands in the same environment where you are running the script.
