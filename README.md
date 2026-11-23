# Panduan Penggunaan Skrip Shell TripTailor

Proyek ini berisi beberapa skrip shell untuk berbagai tugas perencanaan perjalanan. Berikut adalah penjelasan penggunaan terperinci untuk setiap skrip.

## 📁 Struktur Proyek

```text
TripTailor/
├── agents/                        # Skrip terkait agen perjalanan
│   ├── run_TravelAgent.sh         # Alur kerja lengkap
│   ├── run_presearch.sh           # Mode prapencarian
│   ├── run_TravelAgent_direct.sh  # Mode perencanaan langsung
│   └── combination.sh             # Penggabungan hasil
├── planner/                       # Skrip terkait perencana (planner)
│   ├── run_sole_planning.sh       # Perencanaan mandiri
│   └── combination.sh             # Penggabungan hasil perencanaan
├── eval/                          # Skrip terkait evaluasi
│   └── eval.sh                    # Skrip evaluasi
└── Rm/DeepSpeed-Chat/             # Pelatihan model
    └── run_rm_train.sh            # Pelatihan model reward (reward model)
```

## 🚀 Penjelasan Terperinci Skrip

### 1. `agents/run_TravelAgent.sh`
**Fungsi**: Menjalankan alur kerja agen perjalanan secara lengkap

**Parameter konfigurasi rinci**:
- `INPUT_FILE`: Path file pengujian input
- `INPUT_INFO_FILE`: Path file informasi input
- `OUTPUT_PATH`: Path direktori output
- `MODEL_NAME`: Nama model yang digunakan
- `MODE`: Mode berjalan (default: `workflow`)
- `OUTPUT_INFO_FILE`: Path file informasi output

**Cara penggunaan**:
```bash
cd agents
bash run_TravelAgent.sh
```

**Output**: Menghasilkan hasil perencanaan dan file log di direktori `./outputs/`

---

### 2. `agents/run_presearch.sh`
**Fungsi**: Menjalankan mode prapencarian untuk menyiapkan data bagi perencanaan berikutnya

**Parameter konfigurasi rinci**:
- `INPUT_FILE`: Path file pengujian input
- `INPUT_INFO_FILE`: Path file informasi input
- `OUTPUT_PATH`: Path direktori output
- `OUTPUT_INFO_FILE`: Path file informasi prapencarian output
- `MODEL_NAME`: Nama model yang digunakan
- `MODE`: Mode berjalan (tetap: `presearch`)

**Cara penggunaan**:
```bash
cd agents
bash run_presearch.sh
```

**Output**: Menghasilkan file informasi prapencarian untuk menyediakan data bagi perencanaan langsung berikutnya

---

### 3. `agents/run_TravelAgent_direct.sh`
**Fungsi**: Menjalankan mode perencanaan langsung, menghasilkan rencana berdasarkan informasi hasil prapencarian

**Parameter konfigurasi rinci**:
- `INPUT_FILE`: Path file input
- `OUTPUT_PATH`: Path direktori output
- `MODEL_NAME`: Nama model yang digunakan
- `MODE`: Mode berjalan (tetap: `direct`)
- `API_KEY`: Kunci API (API key)
- `BASE_URL`: URL dasar API (API base URL)

**Cara penggunaan**:
```bash
cd agents
bash run_TravelAgent_direct.sh
```

**Prasyarat**: Perlu menjalankan `run_presearch.sh` terlebih dahulu untuk menghasilkan data input

---

### 4. `agents/combination.sh`
**Fungsi**: Menggabungkan hasil output dari agen perjalanan

**Parameter konfigurasi rinci**:
- `INPUT_FILE`: Path file input
- `OUTPUT_DIR`: Path direktori output
- `MODEL_NAME`: Nama model
- `SET_TYPE`: Jenis dataset (default: `test`)
- `STRATEGY`: Jenis strategi (default: `direct`)
- `SUBMISSION_FILE`: Path file hasil akhir

**Cara penggunaan**:
```bash
cd agents
bash combination.sh
```

**Output**: Menghasilkan file hasil akhir

---

### 5. `planner/run_sole_planning.sh`
**Fungsi**: Menjalankan perencana mandiri (sole planner), mendukung berbagai strategi

**Parameter konfigurasi rinci**:
- `INPUT_FILE`: Path file input
- `SET_TYPE`: Jenis dataset (default: `test`)
- `MODEL_NAME`: Nama model yang digunakan
- `OUTPUT_DIR`: Path direktori output
- `STRATEGY`: Strategi perencanaan

**Pengaturan variabel lingkungan (environment variables)**:
- `OPENAI_API_KEY`: Diatur ke kunci API
- `OPENAI_BASE_URL`: Diatur ke URL dasar API

**Strategi yang didukung**:
- `direct`: Perencanaan langsung
- `cot`: Perencanaan Chain of Thought (rantai pemikiran)
- `react`: Perencanaan Reasoning and Acting (penalaran dan tindakan)
- `reflexion`: Perencanaan refleksif (reflexion)

**Cara penggunaan**:
```bash
cd planner
bash run_sole_planning.sh
```

**Output**: Menghasilkan file hasil perencanaan di direktori `./outputs/test/`

---

### 6. `planner/combination.sh`
**Fungsi**: Menggabungkan hasil output dari perencana (planner)

**Parameter konfigurasi rinci**:
- `OUTPUT_DIR`: Path direktori output
- `MODEL_NAME`: Nama model
- `SET_TYPE`: Jenis dataset
- `STRATEGY`: Strategi
- `SUBMISSION_FILE`: Nama file hasil akhir

**Parameter skrip Python**:
- `--query_data_path`: Path data kueri
- `--selected_data_path`: Path data mentah pengujian

**Cara penggunaan**:
```bash
cd planner
bash combination.sh
```

**Prasyarat**: Perlu menjalankan `run_sole_planning.sh` terlebih dahulu

---

### 7. `eval/eval.sh`
**Fungsi**: Mengevaluasi rencana perjalanan yang dihasilkan

**Parameter konfigurasi rinci**:
- `MODEL_NAME`: Nama model yang dievaluasi
- `STRATEGY`: Strategi yang dievaluasi
- `INPUT_FILE`: Path file hasil yang akan dievaluasi
- `INFO_FILE`: Path file informasi
- `FINAL_INFO_FILE`: Path file informasi rencana referensi
- `EVAL_MODEL_NAME`: Nama model yang digunakan untuk evaluasi
- `RM_FILE`: Path file hasil skor model reward

**Cara penggunaan**:
```bash
cd eval
bash eval.sh
```

**Output**: Menghasilkan laporan evaluasi dan file log

---

### 8. `Rm/DeepSpeed-Chat/run_rm_train.sh`
**Fungsi**: Melatih model reward

**Cara penggunaan**:
```bash
cd Rm/DeepSpeed-Chat
bash run_rm_train.sh
```

**Output**: Menghasilkan model yang telah dilatih dan log pelatihan di direktori yang ditentukan

## ⚙️ Konfigurasi Umum

### Variabel Lingkungan
Beberapa skrip memerlukan pengaturan variabel lingkungan:
```bash
export OPENAI_API_KEY="your_api_key"
export OPENAI_BASE_URL="your_base_url"
```

## 🔄 Alur Kerja Umum

### Alur Perencanaan Langsung Lengkap:
```bash
# 1. Prapencarian untuk menyiapkan data
cd agents
bash run_presearch.sh

# 2. Jalankan perencanaan langsung
bash run_TravelAgent_direct.sh

# 3. Gabungkan hasil
bash combination.sh

# 4. Evaluasi hasil
cd ../eval
bash eval.sh
```

### Alur Planner Mandiri:
```bash
# 1. Jalankan perencana (planner)
cd planner
bash run_sole_planning.sh

# 2. Gabungkan hasil
bash combination.sh

# 3. Evaluasi
cd ../eval
bash eval.sh
```

### Alur Pelatihan Model:
```bash
# Latih model reward
cd Rm/data
python data_transform.py
cd ../DeepSpeed-Chat
bash run_rm_train.sh
```

## 🛠️ Konfigurasi Kustom

Parameter konfigurasi setiap skrip berada di bagian atas file dan dapat diubah sesuai kebutuhan:

### Hal yang Umum Diubah:
1. **Konfigurasi model**: Ubah `MODEL_NAME` untuk memilih model yang berbeda
2. **Konfigurasi API**: Ubah `API_KEY` dan `BASE_URL` untuk menggunakan API yang berbeda
3. **Path file**: Ubah path file input dan output
4. **Pemilihan strategi**: Ubah `STRATEGY` untuk memilih strategi perencanaan yang berbeda
