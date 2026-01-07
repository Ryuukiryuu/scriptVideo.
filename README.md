Berikut versi **dirapikan dan terstruktur rapi dalam format Markdown**, **tanpa mengubah isi, urutan, maupun satu karakter pun dari kode dan penjelasan**. Anda dapat **langsung paste ke GitHub (README.md)**.

---

## 📦 LANGKAH 1: IMPORT LIBRARY

```python
# Baris 1-10
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import os

# Setup untuk visualisasi
plt.style.use('seaborn-v0_8-darkgrid')
sns.set_palette("husl")
plt.rcParams['figure.figsize'] = (12, 8)
plt.rcParams['font.size'] = 12
```

**Tunjuk dan jelaskan:**

> Di sini kita import library yang dibutuhkan: **pandas** untuk data, **numpy** untuk numerik, **matplotlib** dan **seaborn** untuk visualisasi.

---

## 📂 LANGKAH 2: LOAD DATA

```python
# Baris 15-25
print("📂 LANGKAH 1: MEMUAT DATA")
print("-" * 40)

try:
    df = pd.read_csv('data/Zomato-data-.csv')
    print("✅ Data berhasil dimuat")
    print(f"📊 Shape data: {df.shape}")
    print(f"📝 Kolom: {list(df.columns)}")
```

**Tunjuk dan jelaskan:**

> Kita load dataset dari file CSV dan tampilkan informasi dasarnya.

---

## 🧹 LANGKAH 3: CLEANING DATA

```python
# Baris 28-38 (Fungsi clean_rate)
def clean_rate(value):
    """Membersihkan kolom rate dari format 'X.X/5' menjadi float"""
    if pd.isna(value):
        return np.nan
    
    value_str = str(value)
    if '/' in value_str:
        value_str = value_str.split('/')[0]
    
    try:
        return float(value_str.strip())
    except:
        return np.nan
```

```python
# Baris 40-45 (Penerapan fungsi)
print("1. Membersihkan kolom 'rate'...")
df['rate_original'] = df['rate']  # Simpan original
df['rate'] = df['rate'].apply(clean_rate)
```

**Tunjuk dan jelaskan:**

> Kolom rating awalnya `'4.1/5'`, kita bersihkan jadi angka **float**.

---

## 📊 BAGIAN 1: DISTRIBUSI JENIS RESTORAN

```python
# Baris 60-90 (UNTUK DISTRIBUSI JENIS RESTORAN)
print("1. Analisis Distribusi Jenis Restoran...")
fig1, ax1 = plt.subplots(figsize=(14, 7))
type_counts = df['listed_in(type)'].value_counts()

bars = ax1.bar(type_counts.index, type_counts.values, color='skyblue', edgecolor='black')
ax1.set_xlabel('Jenis Restoran', fontsize=14, fontweight='bold')
ax1.set_ylabel('Jumlah Restoran', fontsize=14, fontweight='bold')
ax1.set_title('Distribusi Jenis Restoran pada Platform Zomato', 
              fontsize=16, fontweight='bold', pad=20)

# Tambah nilai di atas bar
for bar in bars:
    height = bar.get_height()
    ax1.text(bar.get_x() + bar.get_width()/2., height + 0.5,
            f'{int(height)}', ha='center', va='bottom', fontsize=10)

plt.xticks(rotation=45, ha='right')
plt.tight_layout()
save_plot(fig1, 'restaurant_type_distribution.png')
```

**Tunjuk dan jelaskan:**

> Kode ini membuat **bar chart** untuk melihat jenis restoran apa yang paling banyak.

---

## 📊 BAGIAN 2: VOTES BERDASARKAN JENIS

```python
# Baris 92-120 (UNTUK VOTES PER JENIS)
print("\n2. Analisis Votes Berdasarkan Jenis Restoran...")
votes_by_type = df.groupby('listed_in(type)')['votes'].sum().sort_values(ascending=False)

fig2, ax2 = plt.subplots(figsize=(14, 7))
ax2.plot(votes_by_type.index, votes_by_type.values, 
        marker='o', linewidth=3, markersize=8, color='green', markerfacecolor='red')
ax2.set_xlabel('Jenis Restoran', fontsize=14, fontweight='bold')
ax2.set_ylabel('Total Votes', fontsize=14, fontweight='bold')
ax2.set_title('Total Votes Berdasarkan Jenis Restoran', 
              fontsize=16, fontweight='bold', pad=20)
```

**Tunjuk dan jelaskan:**

> Kode ini menghitung **total votes per kategori** dan membuat **line plot**.

---

## 🏆 BAGIAN 3: RESTORAN POPULER

```python
# Baris 122-135 (UNTUK RESTORAN TERPOPULER)
print("\n3. Identifikasi Restoran Populer...")
max_votes_idx = df['votes'].idxmax()
top_restaurant = df.loc[max_votes_idx]

print(f"   🏆 Restoran dengan votes tertinggi:")
print(f"      Nama: {top_restaurant['name']}")
print(f"      Votes: {top_restaurant['votes']:,}")
```

**Tunjuk dan jelaskan:**

> Kode ini mencari restoran dengan votes tertinggi menggunakan `.idxmax()`.

---

## 📱 BAGIAN 4: PESANAN ONLINE

```python
# Baris 137-165 (UNTUK ANALISIS ONLINE ORDER)
print("\n4. Analisis Ketersediaan Pesanan Online...")
online_counts = df['online_order'].value_counts()

fig3, ax3 = plt.subplots(figsize=(10, 8))
colors = ['#FF6B6B', '#4ECDC4']
wedges, texts, autotexts = ax3.pie(online_counts.values, 
                                   labels=online_counts.index,
                                   autopct='%1.1f%%',
                                   startangle=90,
                                   colors=colors,
                                   explode=(0.05, 0))
```

**Tunjuk dan jelaskan:**

> Kode ini membuat **pie chart** untuk melihat persentase restoran dengan layanan online.

---

## ⭐ BAGIAN 5: DISTRIBUSI RATING

```python
# Baris 167-215 (UNTUK DISTRIBUSI RATING)
print("\n5. Analisis Distribusi Rating...")
fig4, ax4 = plt.subplots(figsize=(12, 7))

n_bins = 15
hist_data = df['rate'].dropna()

n, bins, patches = ax4.hist(hist_data, bins=n_bins, 
                           color='lightblue', 
                           edgecolor='black', 
                           alpha=0.7)
```

**Tunjuk dan jelaskan:**

> Kode ini membuat **histogram** untuk melihat sebaran rating restoran.

---

## 💰 BAGIAN 6: DISTRIBUSI BIAYA

```python
# Baris 217-260 (UNTUK DISTRIBUSI BIAYA)
print("\n6. Analisis Distribusi Biaya...")
fig5, ax5 = plt.subplots(figsize=(14, 7))

cost_data = df['approx_cost(for two people)'].dropna()

# Group biaya ke dalam kategori
bins = [0, 200, 400, 600, 800, 1000, 1200]
labels = ['<200', '200-400', '400-600', '600-800', '800-1000', '1000+']
cost_categories = pd.cut(cost_data, bins=bins, labels=labels, right=False)
```

**Tunjuk dan jelaskan:**

> Kode ini mengelompokkan biaya ke dalam kategori dan membuat bar chart.

---

## 📊 BAGIAN 7: RATING ONLINE vs OFFLINE

```python
# Baris 262-315 (UNTUK PERBANDINGAN RATING)
print("\n7. Perbandingan Rating: Online vs Offline...")
fig6, ax6 = plt.subplots(figsize=(12, 8))

# Filter data
online_data = df[df['online_order'] == 'Yes']['rate'].dropna()
offline_data = df[df['online_order'] == 'No']['rate'].dropna()

# Buat boxplot
box_data = [online_data, offline_data]
box = ax6.boxplot(box_data, labels=['Online', 'Offline'],
                 patch_artist=True,
                 medianprops={'color': 'red', 'linewidth': 2},
                 boxprops={'facecolor': 'lightblue', 'alpha': 0.7})
```

**Tunjuk dan jelaskan:**

> Kode ini membuat **boxplot** untuk membandingkan rating antara pesanan online dan offline.

---

## 🔥 BAGIAN 8: HEATMAP

```python
# Baris 317-350 (UNTUK HEATMAP)
print("\n8. Analisis Heatmap: Jenis vs Online Order...")
pivot_table = df.pivot_table(
    index='listed_in(type)',
    columns='online_order',
    aggfunc='size',
    fill_value=0
)

fig7, ax7 = plt.subplots(figsize=(12, 8))
sns.heatmap(pivot_table, 
            annot=True, 
            fmt='d',
            cmap='YlOrRd',
            cbar_kws={'label': 'Jumlah Restoran'},
            linewidths=0.5,
            linecolor='gray',
            ax=ax7)
```

**Tunjuk dan jelaskan:**

> Kode ini membuat **heatmap** untuk melihat hubungan antara jenis restoran dan ketersediaan online.

---

## 💡 BAGIAN 9: KESIMPULAN

```python
# Baris 352-420 (UNTUK INSIGHT DAN KESIMPULAN)
print("\n💡 LANGKAH 4: KESIMPULAN DAN INSIGHT")
print("-" * 40)

insights = f"""
📊 HASIL ANALISIS DATA ZOMATO
=============================

📈 STATISTIK UMUM:
• Total restoran: {len(df):,}
• Jenis restoran: {len(type_counts)} kategori
• Rating rata-rata: {mean_rating:.2f}
"""
```

**Tunjuk dan jelaskan:**

> Di bagian akhir, kita simpan semua insight ke dalam file teks dan menampilkan kesimpulan hasil analisis.

---

Jika Anda ingin, saya juga bisa:

* Menyesuaikan format agar **lebih akademik (laporan kuliah)**
* Mengubah menjadi **README profesional GitHub**
* Menyusun **narasi laporan praktikum** tanpa mengubah kode

Tinggal beri instruksi lanjutannya.
