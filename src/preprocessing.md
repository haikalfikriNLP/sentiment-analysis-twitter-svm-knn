# Preprocessing Data

Tahapan preprocessing dilakukan untuk meningkatkan kualitas data
sebelum proses pemodelan.

Langkah-langkah yang dilakukan:
1. Cleaning data (menghapus URL, simbol, emoji, angka)
2. Case folding
3. Normalisasi Kata
4. Translate
5. Labeling Vader

Tahapan ini bertujuan untuk mengurangi noise
dan meningkatkan representasi fitur teks.

# Cleaning Data
import re
import string
import nltk

## Remove URL
def remove_URL(tweet):
    if tweet is not None and isinstance(tweet, str):
        url = re.compile(r'https?://\S+|www\.\S+')
        return url.sub(r'', tweet)
    else:
        return tweet

## Remove HTML
def remove_html(tweet):
    if tweet is not None and isinstance(tweet, str):
        html = re.compile(r'<.*?>')
        return html.sub(r'',tweet)
    else:
        return tweet

## Remove Emoji (Biarkan ! dan ? untuk vader)
def remove_emojis(tweet):
    if tweet is not None and isinstance(tweet, str):
        emoji_pattern = re.compile("["
            u"\U0001F600-\U0001F64F"  - emoticons
            u"\U0001F300-\U0001F5FF"  - symbols & pictographs
            u"\U0001F680-\U0001F6FF"  - transport & map symbols
            u"\U0001F700-\U0001F77F"  - alchemical symbols
            u"\U0001F780-\U0001F7FF"  - Geometric Shapes Extended
            u"\U0001F800-\U0001F8FF"  - Supplemental Arrows-C
            u"\U0001F900-\U0001F9FF"  - Supplemental Symbols and Pictographs
            u"\U0001FA70-\U0001FAFF"  - Symbols and Pictographs Extended-A
            u"\U0001FA00-\U0001FA6F"  - Symbols for Legacy Computing / Chess Symbols
            u"\U00002500-\U00002BEF"  - chinese char
            u"\U0001F004-\U0001F0CF"  - Additional emoticons
            u"\U0001F1E0-\U0001F1FF"  - flags (iOS)
            "]+", re.UNICODE)
        return emoji_pattern.sub(r'', tweet)
    else:
        return tweet

## Remove Angka
def remove_numbers(tweet):
    tweet = re.sub(r'\d+', '', tweet)
    return tweet

def remove_symbols(tweet):
    - Menghapus semua simbol kecuali huruf, angka, spasi, tanda seru (!), dan tanda tanya (?)
    tweet = re.sub(r'[^a-zA-Z0-9\s!?]', '', tweet)
    return tweet

## Remove Username
def remove_username(tweet):
    if tweet is not None and isinstance(tweet, str):
        username = re.compile(r'@\w+')
        return username.sub('', tweet)
    else:
        return tweet

df['cleaning'] = df['full_text'].apply(lambda x: remove_URL(x))
df['cleaning'] = df['cleaning'].apply(lambda x: remove_html(x))
df['cleaning'] = df['cleaning'].apply(lambda x: remove_emojis(x))
df['cleaning'] = df['cleaning'].apply(lambda x: remove_numbers(x))
df['cleaning'] = df['cleaning'].apply(lambda x: remove_username(x))
df['cleaning'] = df['cleaning'].apply(lambda x: remove_symbols(x))
df.head(5)

# Case Folding
def case_folding(text):
  if isinstance(text, str):
    lowercase_text = text.lower()
    return lowercase_text
  else:
    return text

df['case_folding'] = df['cleaning'].apply(case_folding)
df.head(5)

# Normalisasi Kata Formal dan Informal
import pandas as pd

## Baca kamus kata baku dari file Excel
kamus_data = pd.read_excel('kamuskatabaku.xlsx')

## Konversi kamus menjadi dictionary untuk pencarian cepat
kamus_tidak_baku = {str(k).strip().lower(): str(v).strip().lower() for k, v in zip(kamus_data['tidak_baku'], kamus_data['kata_baku'])}

## Fungsi untuk mengganti kata tidak baku dengan kata baku
def replace_taboo_words(text, kamus_tidak_baku):
    if not isinstance(text, str):
        return "", [], []

    words = text.split()
    replaced_words = []
    kata_baku = []
    kata_tidak_baku = []

    for word in words:
        word_lower = word.lower()
        if word_lower in kamus_tidak_baku:
            baku_word = kamus_tidak_baku[word_lower]
            replaced_words.append(baku_word)
            kata_baku.append(baku_word)
            kata_tidak_baku.append(word)
        else:
            replaced_words.append(word)

    replaced_text = ' '.join(replaced_words)
    return replaced_text, kata_baku, kata_tidak_baku

## Terapkan fungsi penggantian kata tidak baku
df['normalisasi'], df['kata_baku'], df['kata_tidak_baku'] = zip(
    *df['case_folding'].astype(str).apply(lambda x: replace_taboo_words(x, kamus_tidak_baku))
)

## Buat DataFrame baru hanya dengan kolom yang dibutuhkan
data = pd.DataFrame(df[['created_at','username','full_text']])
df_subset = df.iloc[5:10]
df_subset.head(5)

# Translate Kata
from googletrans import Translator
import time
Buat objek translator sekali saja di luar loop
translator = Translator()

## Fungsi untuk menerjemahkan teks dan mengonversinya ke huruf kecil
def translate_text(text, target_language='en'):
    if text is None or text == '':
        return ''  - Kembalikan string kosong jika teks kosong
    translation = translator.translate(text, dest=target_language)
    return translation.text.lower()  - Konversi hasil ke lowercase

## Iterasi melalui kolom "normalisasi" dan menerjemahkan teks
translated_data = []

for index, row in df.iterrows():
    try:
        translated_text = translate_text(row['normalisasi'])
        translated_data.append(translated_text)
        time.sleep(1)  - delay 1 detik antar request untuk menghindari timeout
    except Exception as e:
        print(f"Error di index {index}: {e}")
        translated_data.append('')  - Isi dengan kosong kalau gagal translate

## Tambahkan kolom terjemahan ke DataFrame
df['translated_data'] = translated_data

## Menampilkan beberapa hasil
df[['normalisasi', 'translated_data']].head(10)

# VADER Labeling
## Inisialisasi SentimentIntensityAnalyzer
sia = SentimentIntensityAnalyzer()

## Hitung skor sentimen untuk setiap teks
def calculate_sentiment_scores(text):
    return sia.polarity_scores(text)

## Terapkan fungsi ke kolom 'translated_stemming_data'
sentiment_scores = data['translated_data'].apply(calculate_sentiment_scores)

## Ekstrak skor positif, negatif, netral, dan compound
data['Positive'] = sentiment_scores.apply(lambda x: x['pos'])
data['Negative'] = sentiment_scores.apply(lambda x: x['neg'])
data['Neutral'] = sentiment_scores.apply(lambda x: x['neu'])
data['Compound'] = sentiment_scores.apply(lambda x: x['compound'])

## Kategorikan sentimen berdasarkan skor compound
def categorize_sentiment(compound_score):
    if compound_score >= 0.05:
        return 'Positive'
    elif compound_score <= -0.05:
        return 'Negative'
    else:
        return 'Neutral'

data['Sentiment'] = data['Compound'].apply(categorize_sentiment)

## Hitung jumlah masing-masing sentimen
sentiment_counts = data['Sentiment'].value_counts()

## Tampilkan 10 baris pertama dan jumlah sentimen
print("\nJumlah Sentimen:")
print(sentiment_counts)
