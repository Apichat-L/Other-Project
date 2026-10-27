# 📧 Maily Detector

A Chrome extension that scans the email you have open in Gmail and uses a fine-tuned **DistilBERT** model to classify it as **SPAM**, **NOT SPAM**, or **SUBSCRIPTION**.

## Features

- One-click scan of the currently open Gmail email (subject + body)
- Three categories, each with its own color:
  - 🟢 **NOT SPAM** – looks like a legitimate message
  - 🔴 **SPAM** – shows patterns common in spam or phishing
  - 🔵 **SUBSCRIPTION** – a subscription or newsletter email, with steps for cancelling
- Shows a confidence percentage and the reason for the result
- Warns you when an email is mostly Thai, since the model was trained on English, and lets you scan anyway

## The model

The classifier is `distilbert-base-uncased` fine-tuned with **LoRA** on three Kaggle datasets:

- [Spam Email Dataset](https://www.kaggle.com/datasets/jackksoncsie/spam-email-dataset)
- [Spam Assassin Email Classification Dataset](https://www.kaggle.com/datasets/ganiyuolalekan/spam-assassin-email-classification-dataset)
- [Subscription Renewal and Cancellation Dataset](https://www.kaggle.com/datasets/mirzayasirabdullah07/subscription-renewal-and-cancellation-dataset)

Validation results after 3 epochs:

| Metric | Score |
|---|---|
| Accuracy | 98.6% |
| F1 (weighted) | 98.6% |

| Class | Precision | Recall | Support |
|---|---|---|---|
| NOT SPAM | 0.99 | 0.99 | 636 |
| SPAM | 0.96 | 0.96 | 137 |
| SUBSCRIPTION | 1.00 | 1.00 | 12 |

The SUBSCRIPTION class has only 12 validation samples, so its score should be read with caution.

The full training code is in [`model/Spam_Mail_Detector_BERT.ipynb`](model/Spam_Mail_Detector_BERT.ipynb) (built for Google Colab).

## How it works

1. `content.js` runs inside Gmail and reads the subject and body of the open email (trimmed to 2,000 characters).
2. When you click **Scan**, `popup.js` sends the text to the model hosted on a Hugging Face Space through the Gradio API.
3. The result is shown in the popup with the label, a confidence bar and an explanation.

## Tech stack

- **Model:** Python, PyTorch, Hugging Face Transformers, PEFT (LoRA), DistilBERT
- **Extension:** JavaScript, HTML, CSS, Chrome Extension (Manifest V3)
- **Hosting:** Hugging Face Spaces + Gradio API

## Project structure

```
maily-detector/
├── manifest.json        # Extension config and permissions
├── content.js           # Reads the open email from the Gmail page
├── popup.html           # Popup UI
├── popup.js             # Calls the model and displays the result
├── model/
│   └── Spam_Mail_Detector_BERT.ipynb   # Data prep, LoRA fine-tuning, evaluation
└── docs/
    ├── Maily Detector Final Presentation.pdf
    ├── Software_Requirements_Specification.pdf
    └── Software_Design_Specification.pdf
```

## Installing the extension

1. Download or clone this repository.
2. Open Chrome and go to `chrome://extensions`.
3. Turn on **Developer mode** (top right).
4. Click **Load unpacked** and select the `maily-detector` folder.
5. Pin the extension from the puzzle-piece icon in the toolbar.

## Usage

1. Open [Gmail](https://mail.google.com) and click on an email to open it.
2. Click the Maily Detector icon.
3. Press **Scan** and wait for the result.

If you get an error, refresh the Gmail tab and try again. The Hugging Face Space may need a moment to wake up if it hasn't been used for a while.

## Training the model yourself

1. Open the notebook in Google Colab and run the cells in order.
2. Cell 2 asks you to upload your own `kaggle.json` (download it from your Kaggle account under **Settings → API**).
3. The Flask/ngrok cell needs your own ngrok auth token, set as the `NGROK_AUTHTOKEN` environment variable.

## Privacy note

The text of the email you scan is sent to the Hugging Face Space for classification. Emails are only read when you press **Scan**, never in the background.

## Limitations

- Trained on English emails; results on Thai or other languages may be inaccurate.
- Depends on Gmail's page structure, so a Gmail layout change could stop it from reading emails.
- Requires the Hugging Face Space to be online.

## Team

Group 10 – SIIT, Thammasat University
