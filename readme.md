# 📰 Company News Bot

An AI-powered chatbot that gives you real-time summaries of the **latest news about any company**. Just type a question like "What's new with Apple and Google?" and get clean, concise updates.

> ✅ Built using only **free, open-source tools**.  
> ✅ No paid APIs.  
> ✅ Web UI powered by **Gradio**.

---

## 🚀 Features

- 🔎 **Real-time news search** using web scraping and Google Search
- 🧠 **Summarized articles** using HuggingFace `distilbart-cnn-12-6`
- 🤖 Handles **greetings**, **casual language**, and **typos**
- ✨ Choose output style: Formal, Casual, or Bullet Points
- 🧩 Modular architecture with multiple AI agents:
  - SearchAgent
  - ScrapeAgent
  - SummarizerAgent
  - ChatAgent (for parsing, NER, fuzzy matching)

---

## 🔧 Tools & Libraries Used

| Component        | Tool/Model                              |
|------------------|------------------------------------------|
| UI               | Gradio                                   |
| Search           | `googlesearch-python`                    |
| Scraping         | `requests`, `beautifulsoup4`             |
| Summarization    | `sshleifer/distilbart-cnn-12-6` (Hugging Face Transformers) |
| NLP / NER        | `spaCy` with `en_core_web_trf`           |

---

## 🔧 Installation (Colab or Local)

Run the following to install all dependencies:

bash
pip install -q  \
  gradio \
  googlesearch-python \
  beautifulsoup4 \
  requests \
  transformers \
  spacy[transformers]

python -m spacy download en_core_web_trf
Restart the session if run in Google Collab.

---

## ▶ Run the App

Just execute the Python file or run all cells in the Colab notebook:

python
python app.py


Or, launch in Colab with gr.Interface().launch(debug=True).

---

## 💬 Example Queries

> “Give me latest news about Zoho and Infosys”  
> “What’s up with Apple?”  
> “Show me updates from Google, Meta, and Tesla”  
> “Hi” → Normal greetings supported  
> “Tell me about news” → Handled as non-company query

---

## 🎯 How It Works

### 1. User Input → NLP → Company Names
- Extracts company names using spaCy NER
- Applies fuzzy matching against KNOWN_COMPANIES list
- Filters out generic words like “news”, “report”, “give”

### 2. Company → Google Search → Top 3 Links
- googlesearch-python used to get latest links for that company
- Not restricted to 1-day or 7-day filters — fetches current latest news results

### 3. Scraping & Summarization
- Scrapes article paragraphs using BeautifulSoup
- Summarizes using sshleifer/distilbart-cnn-12-6 via Hugging Face Transformers
- Summaries styled as Formal, Casual, or Bullet Points

### 4. Chat Response
- Summarized news displayed per company
- If content fails, links are shown under "📎 Additional Links"

---

## 🧭 Architecture Diagram

```
+----------------------+
|  User Input (Gradio) |
+----------+-----------+
           |
           v
+----------------------+       +----------------------+
|      ChatAgent       |<----->|   Known Companies DB |
|----------------------|       +----------------------+
| - Detects intent     |
| - Extracts company   |
| - Fuzzy matches name |
+----------+-----------+
           |
           v
+----------------------+
|    SearchAgent       |
|----------------------|
| - Google search top  |
|   3 links for:       |
|   "Company latest news" |
+----------+-----------+
           |
           v
+----------------------+
|    ScrapeAgent       |
|----------------------|
| - Extract article    |
|   text (BeautifulSoup)|
+----------+-----------+
           |
           v
+-----------------------------+
|      SummarizerAgent       |
|-----------------------------|
| - Summarizes using         |
|   HuggingFace transformers |
| - Supports 3 styles:       |
|   Formal, Casual, Bullets  |
+----------+------------------+
           |
           v
+-----------------------------+
|       Gradio Chat UI       |
|-----------------------------|
| - Display company-wise news|
| - Show summary + link      |
| - Show fallback links      |
+-----------------------------+

```


---
## 📦 Project Structure


├── app.py               # Main script (everything in one file)

├── README.md            # This file

