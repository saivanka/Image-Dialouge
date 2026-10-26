# Image-Dialouge

Visual question answering: upload an image and ask a question about it, and the app answers using the ViLT visual-question-answering model (`dandelin/vilt-b32-finetuned-vqa`). The repo ships two front ends for the same model — a Streamlit app and a FastAPI service.

## What it does

- **Streamlit app (`app.py`)** — titled “Image-Dialogue”: upload a JPG/JPEG/PNG image, type a question, press Submit, and the answer is shown. The image is converted to PNG bytes and passed to `get_answer()`, which runs the ViLT processor and model and returns the top predicted label.
- **FastAPI service (`api.py`)** — titled “Image-Dialoge”:
  - `GET /` redirects to the API docs (`/docs`).
  - `POST /result` accepts an uploaded image (`file_image`) and a question (`input_text`) and returns `{"Answer": "..."}` (or an error message on failure).

## Tech stack

- Python, PyTorch + Hugging Face `transformers` (ViLT processor and `ViltForQuestionAnswering`)
- Streamlit for the web UI, Pillow for image handling
- FastAPI + Uvicorn for the API (with `python-multipart` for file uploads)

## How to run

```bash
pip install -r requirements.txt
```

Streamlit app:

```bash
streamlit run app.py
```

FastAPI service:

```bash
uvicorn api:app --reload
```

Then open the docs at http://127.0.0.1:8000/docs, or call `POST /result` with an image file and a question.

## Project structure

```
app.py              # Streamlit UI: image upload + question -> ViLT answer
api.py              # FastAPI service: GET / -> /docs, POST /result -> {"Answer": ...}
requirements.txt    # transformers, torch, requests, Pillow, fastapi, uvicorn, streamlit, python-multipart
.idea/              # IDE settings directory
```
