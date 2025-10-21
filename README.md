# README — Hacker News Scraper

---

## 🧾 Full Code

```python
# File: hacker_news_scraper.py
import requests
from bs4 import BeautifulSoup
import pandas as pd

url = 'https://news.ycombinator.com'
response = requests.get(url)
# Agar request fail ho to response.status_code check karna behtar hai
response.raise_for_status()

soup = BeautifulSoup(response.text, 'lxml')

# Lists jo hum data store karne ke liye use karenge
titles = []
links = []

# Hacker News mein har article ka wrapper 'tr' tag hota hai jisme class 'athing' hoti hai
articles = soup.find_all('tr', class_='athing')

for article in articles:
    titleline = article.find('span', class_='titleline')
    if titleline:
        title_tag = titleline.find('a')
        if title_tag:
            title = title_tag.text.strip()
            link = title_tag['href']
            titles.append(title)
            links.append(link)

# Pandas DataFrame bana kar CSV mein save karna
df = pd.DataFrame({
    'Title': titles,
    'Link': links
})

output_file = 'hacker_news_articles.csv'
df.to_csv(output_file, index=False)

print(f"Saved {len(df)} articles to {output_file}")
```

---

# 🧠 Step-by-step Explanation

Yahan neechay har step ka **simple** aur **clear** Roman Urdu mein matlab aur purpose diya gaya hai. Headings follow kar ke agar koi sirf kisi ek hisse ko dekhna chahta ho to woh seedha jump kar sakta hai.

---

## 🔹 Step 1: Libraries import karna

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
```

* **requests**: Yeh library HTTP requests bhejne ke liye hai — matlab website ka HTML lene ke liye.
* **BeautifulSoup**: Yeh HTML ko parse karta hai taake hum usme se specific tags (jaise titles, links) nikal saken.
* **pandas**: Yeh data ko table (DataFrame) mein rakhne aur CSV file banane ke liye use hota hai.

---

## 🔹 Step 2: Website se HTML lena

```python
url = 'https://news.ycombinator.com'
response = requests.get(url)
response.raise_for_status()
soup = BeautifulSoup(response.text, 'lxml')
```

* `requests.get(url)` se tum Hacker News ka main page download kar rahe ho.
* `response.raise_for_status()` ensure karta hai agar website se kuch error aaya to script ruk jaye aur error dikhaye (yeh debugging ke liye useful hai).
* `BeautifulSoup(..., 'lxml')` HTML ko parse karta hai taake aage usme se specific tags find kar sakein.

---

## 🔹 Step 3: Khali lists banana

```python
titles = []
links = []
```

* Hum `titles` aur `links` lists bana rahe hain jahan har article ka title aur uska link store hoga.
* Yeh lists baad mein DataFrame mein convert hongi.

---

## 🔹 Step 4: Articles dhundhna (HTML structure samajhna)

```python
articles = soup.find_all('tr', class_='athing')
```

* Hacker News ka HTML structure typically is tarah hota hai ke har article ek `tr` element mein hota hai jiska class `athing` hota hai.
* `find_all` se hum saare aise `tr` elements le lete hain.

---

## 🔹 Step 5: Har article ka title aur link nikalna

```python
for article in articles:
    titleline = article.find('span', class_='titleline')
    if titleline:
        title_tag = titleline.find('a')
        if title_tag:
            title = title_tag.text.strip()
            link = title_tag['href']
            titles.append(title)
            links.append(link)
```

* Har `article` ke andar `span` tag jisme `class='titleline'` hoti hai, woh title ka container hota hai.
* Uske andar `a` tag hota hai jisme actual text (title) aur `href` attribute (link) hota hai.
* `title_tag.text.strip()` se title ka extra whitespace remove kar dete hain.
* Phir title aur link lists mein add kar dete hain.

---

## 🔹 Step 6: DataFrame banana aur CSV mein save karna

```python
df = pd.DataFrame({
    'Title': titles,
    'Link': links
})

output_file = 'hacker_news_articles.csv'
df.to_csv(output_file, index=False)
```

* `pandas.DataFrame` se hum lists ko columns mein convert kar dete hain.
* `to_csv(..., index=False)` CSV file bana deta hai bina extra index column ke.

---

## 🔹 Step 7: Confirmation print karna

```python
print(f"Saved {len(df)} articles to {output_file}")
```

* Yeh sirf terminal par bata dega ke kitne articles save hue.

---

# 🔧 Extra Tips & Notes

* **Relative links:** Kabhi-kabhi `href` relative hota hai (e.g. `/item?id=12345`). Agar tum chahte ho ke full URL milay to check kar lo aur agar link `http` se start na ho to `https://news.ycombinator.com` prefix kar do.

* **Rate limiting / politeness:** Agar tum bohot zyada pages scrape kar rahe ho to server pe burden na paonchane ke liye `time.sleep()` use karo. Aur terms of service ka khayal rakho.

* **Selectors change ho sakte hain:** Websites ka HTML kabhi bhi change ho sakta hai; agar script kaam na kare to inspect karke correct tags/classes identify karo.

* **Error handling improve karna:** `try/except` blocks add kar ke requests ko robust banao.

---

## ✅ Summary

Yeh README tumhe ek **ready-to-run** Python script deta hai jo Hacker News se titles aur links scrap karke `hacker_news_articles.csv` bana dega.
---

**Author:** Nouman Sami
