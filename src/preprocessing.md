# Preprocessing Data

Tahapan preprocessing dilakukan untuk meningkatkan kualitas data
sebelum proses pemodelan.

Langkah-langkah yang dilakukan:
1. Cleaning data (menghapus URL, simbol, emoji, angka)
2. Case folding
3. Normalisasi Kata
4. Translate
5. Labeling VADER

Tahapan ini bertujuan untuk mengurangi noise
dan meningkatkan representasi fitur teks.

---

## Cleaning Data

```python
import re
import nltk

# Remove URL
def remove_URL(tweet):
    if tweet is not None and isinstance(tweet, str):
        url = re.compile(r'https?://\S+|www\.\S+')
        return url.sub(r'', tweet)
    return tweet

# Remove HTML
def remove_html(tweet):
    if tweet is not None and isinstance(tweet, str):
        html = re.compile(r'<.*?>')
        return html.sub(r'', tweet)
    return tweet

# Remove Emoji (kecuali ! dan ? untuk VADER)
def remove_emojis(tweet):
    if tweet is not None and isinstance(tweet, str):
        emoji_pattern = re.compile("["
            u"\U0001F600-\U0001F64F"
            u"\U0001F300-\U0001F5FF"
            u"\U0001F680-\U0001F6FF"
            u"\U0001F700-\U0001F77F"
            u"\U0001F780-\U0001F7FF"
            u"\U0001F800-\U0001F8FF"
            u"\U0001F900-\U0001F9FF"
            u"\U0001FA00-\U0001FAFF"
            u"\U00002500-\U00002BEF"
            u"\U0001F1E0-\U0001F1FF"
            "]+", re.UNICODE)
        return emoji_pattern.sub(r'', tweet)
    return tweet

# Remove Numbers
def remove_numbers(tweet):
    return re.sub(r'\d+', '', tweet)

# Remove Symbols (kecuali ! dan ?)
def remove_symbols(tweet):
    return re.sub(r'[^a-zA-Z0-9\s!?]', '', tweet)

# Remove Username
def remove_username(tweet):
    if tweet is not None and isinstance(tweet, str):
        username = re.compile(r'@\w+')
        return username.sub('', tweet)
    return tweet
```
## Case Folding

```python
def case_folding(text):
    if isinstance(text, str):
        return text.lower()
    return text
```

## Normalisasi Kata Formal dan Informal

```python
import pandas as pd

kamus_data = pd.read_excel('kamuskatabaku.xlsx')
kamus_tidak_baku = {
    str(k).lower(): str(v).lower()
    for k, v in zip(kamus_data['tidak_baku'], kamus_data['kata_baku'])
}

def replace_taboo_words(text, kamus):
    if not isinstance(text, str):
        return text
    words = text.split()
    return ' '.join([kamus.get(w.lower(), w) for w in words])
```
## Translate Kata
```python
from googletrans import Translator
import time

translator = Translator()

def translate_text(text, target_language='en'):
    if not text:
        return ''
    translation = translator.translate(text, dest=target_language)
    return translation.text.lower()
```

## VADER Labeling
```python
from nltk.sentiment.vader import SentimentIntensityAnalyzer

sia = SentimentIntensityAnalyzer()

def calculate_sentiment(text):
    score = sia.polarity_scores(text)
    if score['compound'] >= 0.05:
        return 'Positive'
    elif score['compound'] <= -0.05:
        return 'Negative'
    else:
        return 'Neutral'
```
