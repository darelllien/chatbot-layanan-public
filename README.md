# 🏛️ E-Pemda Smart Chatbot: Sistem Informasi Layanan Publik Berbasis Dense Semantic Representation & Expert System

Selamat datang di repositori resmi proyek **E-Pemda Smart Chatbot**. Aplikasi ini merupakan sistem asisten pintar interaktif berbasis web yang dirancang khusus untuk mengotomatisasi penyediaan informasi alur prosedur birokrasi dan kependudukan tingkat Pemerintah Daerah (Pemda).

Sistem ini dikembangkan secara end-to-end oleh **Kelompok AI03 - Universitas Cakrawala** untuk memenuhi komponen penilaian Tugas Akhir / UAS Mata Kuliah Artificial Intelligence (Kecerdasan Buatan).

---

## 🎯 Latar Belakang & Batasan Masalah

Layanan informasi publik seringkali menghadapi kendala antrean manual akibat tingginya pertanyaan berulang dari warga terkait syarat kependudukan. Mengandalkan pencarian kata kunci kaku (_Sparse Exact Match_) seringkali gagal menangkap maksud pertanyaan kasual warga yang memiliki variasi bahasa yang sangat beragam.

**E-Pemda Smart Chatbot** hadir dengan mendisrupsi metode konvensional tersebut melalui implementasi **Dense Semantic Representation** (Pemetaan Kedekatan Konteks Makna pada Ruang Vektor) untuk mengenali niat (_intent_) warga secara adaptif, yang kemudian dieksekusi secara presisi menggunakan **Expert System** (Sistem Pakar berbasis aturan deterministik) untuk menghasilkan jawaban regulasi yang valid.

### 📋 Cakupan Informasi (Intent Domains)

1. **`pembuatan_ktp`**: Prosedur pembuatan, penggantian KTP rusak, atau cetak ulang KTP hilang.
2. **`pembuatan_kk`**: Alur pengurusan Kartu Keluarga baru, penambahan anggota, atau pecah KK[cite: 1].
3. **`akta_kelahiran`**: Syarat pembuatan akta lahir baru untuk bayi maupun pengurusan akta hilang[cite: 1].
4. **`kartu_identitas_anak`**: Prosedur penerbitan KIA untuk anak usia 0-5 tahun dan 5-17 tahun[cite: 1].
5. **`pindah_domisili`**: Alur pencabutan berkas dan pengurusan Surat Keterangan Pindah Datang (SKPWNI)[cite: 1].
6. **`salam_bantuan`**: Manajemen sambutan interaktif dan panduan awal penggunaan chatbot[cite: 1].

---

## 🧠 Eksplorasi Teknik AI & Alur Arsitektur

Sistem mengintegrasikan kombinasi matematika ruang vektor dengan logika keputusan linier untuk memproses setiap pertanyaan input pengguna:

````text
┌──────────────┐      ┌────────────────────────┐      ┌────────────────────────┐
│  User Input  │ ──>  │ Text Preprocessing     │ ──>  │ TF-IDF Vectorizer      │
│  (Inquiry)   │      │ (Lowercasing/Stripping)│      │ (Dense Embedding Space)│
└──────────────┘      └────────────────────────┘      └──────────────────┬─────┘
                                                                         │
                                                                         ▼
┌──────────────┐      ┌────────────────────────┐      ┌────────────────────────┐
│ Output UI &  │ <──  │ Expert System Lookup   │ <──  │ Cosine Similarity      │
│ REST API JSON│      │ (If-Then Rule Gateway) │      │ (Proximity Calculation)│
└──────────────┘      └────────────────────────┘      └────────────────────────┘

1. **Lightweight Text Preprocessing**: Pembersihan string ringan lewat pemetaan huruf kecil (*case folding*) dan pemotongan spasi ekstra (*stripping*)[cite: 2, 3]. Pendekatan ini dipilih secara strategis dibanding *stemming* berat guna melindungi integritas kata kunci penting kependudukan (seperti KTP, KK, KIA)[cite: 2, 3].
2. **Dense Representation Space**: Pengestrakan fitur semantik teks menggunakan kombinasi kata dan frasa majemuk melalui `TfidfVectorizer(ngram_range=(1, 2))`. Mengubah teks natural menjadi koordinat matriks bobot yang padat.
3. **Cosine Similarity Proximity**: Mengukur sudut orientasi kedekatan makna antara vektor input warga terhadap matriks data latih:
   $$\text{Cosine Similarity}(\vec{a}, \vec{b}) = \frac{\vec{a} \cdot \vec{b}}{\|\vec{a}\| \|\vec{b}\|}$$
4. **Smart Thresholding Gateway**: Sistem dilengkapi pembatas toleransi kesalahan (Threshold) sebesar **15%**. Jika skor kecocokan tertinggi di bawah batas tersebut, sistem secara otomatis mengategorikannya sebagai `tidak_dikenali` guna menghindari disinformasi birokrasi.
5. **Deterministic Expert System**: Hasil klasifikasi niat yang valid langsung dipetakan ke basis pengetahuan `knowledge_base.json` untuk mengeluarkan dokumen panduan resmi pemerintah daerah.

---

## 📂 Struktur Repositori Proyek
```text
analisis-sentimen-marketplace/
│
├── data/
│   └── knowledge_base.json      # Basis pengetahuan aturan & sampel frasa tanya warga
│
├── models/                      # Folder penyimpanan aset model biner (.pkl)
│   ├── tfidf_vectorizer.pkl     # Objek pengonversi teks ke ruang vektor biner
│   ├── dense_embeddings.pkl     # Matriks koordinat semantik data latih
│   └── intent_mapping.pkl       # Pemetaan indeks label target intent
│
├── notebooks/
│   └── train_chatbot.py         # Pipeline backend latih ulang & uji integritas model
│
├── src/
│   └── app.py                   # Antarmuka web premium & simulator JSON REST API (Streamlit)
│
├── requirements.txt             # Berkas dependensi pustaka untuk deployment cloud
└── README.md                    # Dokumentasi utama proyek

---

## ⚙️ Panduan Instalasi & Eksekusi Lokal

### 1. Kloning Repositori & Masuk ke Direktori
```powershell
git clone [https://github.com/darelllien/chatbot-layanan-public.git](https://github.com/darelllien/chatbot-layanan-public.git)
cd chatbot-layanan-public

# Aktivasi environment (.venv)
.venv\Scripts\activate

# Instalasi seluruh library pendukung
pip install -r requirements.txt

cd notebooks
python train_chatbot.py

cd ..
streamlit run src/app.py

🌐 Tautan Tinjauan Produksi
Aplikasi Live URL: https://chatbot-layanan-public-dtz448ysji9r8utowb65x7.streamlit.app/
Repositori Kode: https://github.com/darelllien/chatbot-layanan-public
````
