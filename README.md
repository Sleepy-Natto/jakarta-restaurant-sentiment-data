# Jakarta Restaurant Reviews Dataset (2021-2026)

A dataset of 14,720 restaurant reviews scraped from Google Reviews of Jakarta restaurants, covering reviews from 2021 to early 2026. This dataset accompanies our paper accepted to the **13th International Conference on Electrical Engineering, Computer Science and Informatics (EECCIS 2026)**.

## 📄 Citation

If you use this dataset in your research, please cite:

```bibtex
@inproceedings{bara2026sentiment,
  title={Sentiment Analysis of Restaurant Reviews: A Comparative Study of Machine Learning Models Using TF-IDF},
  author={Bara, Arya Nathan and Setiawan, Nicholas Ryan Putra and Achmad, Said and Wibowo, Antoni},
  booktitle={2026 13th International Conference on Electrical Engineering, Computer Science and Informatics (EECCIS)},
  year={2026},
  organization={IEEE},
  note={In Press}
}
```

**Paper Repository:** [Sleepy-Natto/jakarta-restaurant-sentiment-data](https://github.com/Sleepy-Natto/jakarta-restaurant-sentiment-data)

---

## 📊 Dataset Overview

- **Total Reviews:** 14,720
- **Date Range:** 2021 - early 2026
- **Source:** Google Reviews (Jakarta restaurants)
- **Format:** CSV with columns: `review_text`, `rating`, `date`, `language`

### Star Rating Distribution
- ⭐⭐⭐⭐⭐ (5 stars): 11,428 reviews (77.6%)
- ⭐⭐⭐⭐ (4 stars): 1,565 reviews (10.6%)
- ⭐⭐⭐ (3 stars): 488 reviews (3.3%)
- ⭐⭐ (2 stars): 221 reviews (1.5%)
- ⭐ (1 star): 612 reviews (4.2%)
- **Missing/irregular ratings:** 406 reviews (2.8%)

---

## ⚠️ Data Quality Notes

This is a **raw, uncleaned dataset** with known data quality issues:

### 1. Missing Review Text (46.5%)
- **6,848 rows** (46.5%) have missing `review_text`
- These are star-only ratings without written reviews
- Many users on Google Reviews rate without writing text

### 2. Irregular Date Format (13.3%)
- **1,960 rows** (13.3%) have irregular or missing dates
- Includes relative dates like "2 months ago" that weren't parsed
- Some entries have no date information

### 3. Highly Imbalanced Classes
- **Positive reviews (4-5 stars):** 88.2%
- **Negative reviews (1-2 stars):** 5.7%
- **Neutral reviews (3 stars):** 3.3%
- This imbalance is typical of public review platforms

---

## 🔧 Recommended Data Cleaning Pipeline

Our paper used the following cleaning steps:

```python
# 1. Remove duplicates and null values
df = df.drop_duplicates()
df = df.dropna(subset=['review_text', 'rating'])

# Result: 14,720 → 13,007 reviews

# 2. Date filtering (keep 2021-2026 only)
df = df[df['date'].dt.year >= 2021]

# Result: 13,007 → 11,564 reviews

# 3. Remove neutral reviews (optional, for binary classification)
df = df[df['rating'].isin([1, 2, 4, 5])]

# Result: 11,564 → 11,212 reviews

# 4. Text preprocessing
# - Tokenization
# - Lowercasing
# - Sastrawi stemming (Indonesian)
# - Token length filtering (3-25 characters)
```

**Final clean dataset:** 11,212 reviews (76.1% of original)

---

## 📁 File Structure

```
jakarta-restaurant-sentiment-data/
├── data/
│   └── jakarta_restaurant_reviews_raw.csv    # Raw dataset (14,720 rows)
├── README.md
├── LICENSE                                     # MIT License
└── CITATION.cff                               # Citation metadata
```

---

## 🚀 Usage Example

```python
import pandas as pd

# Load raw data
df = pd.read_csv('data/jakarta_restaurant_reviews_raw.csv')

# Basic cleaning
df_clean = df.dropna(subset=['review_text', 'rating'])
df_clean = df_clean.drop_duplicates()

# Filter by date (2021-2026)
df_clean['date'] = pd.to_datetime(df_clean['date'], errors='coerce')
df_clean = df_clean[df_clean['date'].dt.year >= 2021]

# Binary sentiment (positive/negative only)
df_binary = df_clean[df_clean['rating'].isin([1, 2, 4, 5])]
df_binary['sentiment'] = df_binary['rating'].apply(lambda x: 'positive' if x >= 4 else 'negative')

print(f"Clean dataset: {len(df_binary)} reviews")
print(df_binary['sentiment'].value_counts())
```

---

## 🧪 Research Use Cases

This dataset is suitable for:

✅ **Sentiment analysis** (binary or multi-class)  
✅ **Class imbalance research** (SMOTE, undersampling, hybrid strategies)  
✅ **Indonesian NLP** (text preprocessing, stemming, tokenization)  
✅ **TF-IDF feature extraction**  
✅ **Machine learning model comparison** (Logistic Regression, SVM, Random Forest, Naive Bayes)  
✅ **Deep learning baselines** (BERT, LSTM, transformers)

---

## ⚖️ Ethical Considerations

- Reviews were scraped from **public Google Reviews** platform
- No personally identifiable information (PII) is included
- Restaurant names are **not** included to avoid targeting specific businesses
- This dataset is for **academic research purposes only**
- Users should comply with Google's Terms of Service when using this data

---

## 🔒 License

This dataset is released under the **MIT License**.

```
MIT License

Copyright (c) 2026 Arya Nathan Bara, Nicholas Ryan Putra Setiawan

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

See [LICENSE](LICENSE) file for full details.

---

## 📧 Contact

**Authors:**
- Arya Nathan Bara - [arya.bara@binus.ac.id](mailto:arya.bara@binus.ac.id)
- Nicholas Ryan Putra Setiawan - [nicholas.setiawan003@binus.ac.id](mailto:nicholas.setiawan003@binus.ac.id)

**Affiliation:** Computer Science Department, Bina Nusantara University, Jakarta, Indonesia

**GitHub:** [@Sleepy-Natto](https://github.com/Sleepy-Natto)

---

## 🙏 Acknowledgments

This dataset was collected as part of research on sentiment analysis of Indonesian restaurant reviews, supervised by Dr. Said Achmad and Dr. Antoni Wibowo at Bina Nusantara University.

---

## 📝 Changelog

- **v1.0.0** (2026): Initial release - Raw dataset of 14,720 Jakarta restaurant reviews (2021-2026)

---

**Last Updated:** January 2026
