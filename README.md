# 1. Mount Google Drive
from google.colab import drive
drive.mount('/content/drive')

# 2. Path folder tempat file berada
import os
folder_path = "/content/drive/MyDrive/Rekap gangguan"

# Cek apakah folder ada
if os.path.exists(folder_path):
    print("Status: Folder ditemukan ✔️")
else:
    print("Status: Folder TIDAK ditemukan ❌ — periksa penamaan folder")
    raise SystemExit()

    import os
import pandas as pd

# 1. Path folder tempat file berada
folder_path = "/content/drive/MyDrive/Rekap gangguan"

# 2. Cek apakah folder ada
if os.path.exists(folder_path):
    print("Status: Folder ditemukan ✔️")
else:
    raise SystemExit("Status: Folder TIDAK ditemukan ❌ - periksa penamaan folder")

# 3. Ambil semua file CSV dalam folder
files = [f for f in os.listdir(folder_path) if f.endswith('.csv')]
print(f"Jumlah file ditemukan: {len(files)}")

# 4. List untuk menampung dataframe
df_list = []

for file in files:
    file_path = os.path.join(folder_path, file)
    df = pd.read_csv(file_path)

    # Hapus kolom '#' jika ada
    if '#' in df.columns:
        df = df.drop(columns=['#'])

    df_list.append(df)

# 5. Gabungkan semua dataframe
df_final = pd.concat(df_list, ignore_index=True)

print("Penggabungan selesai ✔️")
print(f"Total baris setelah digabung: {len(df_final)}")

# 6. Simpan file baru
output_path = "/content/drive/MyDrive/Rekap gangguan 2.csv"
df_final.to_csv(output_path, index=False)

print(f"File berhasil disimpan sebagai: {output_path}")


import pandas as pd

# 1. Baca file Rekap gangguan 2
path_input = "/content/drive/MyDrive/Rekap gangguan 2.csv"
df = pd.read_csv(path_input)

# 2. Ubah kolom Tgl KA menjadi datetime (format hari-bulan-tahun)
df['Tgl KA'] = pd.to_datetime(df['Tgl KA'], dayfirst=True, errors='coerce')

# 3. Buat batas tanggal: 1 Februari 2025 s.d. 31 Oktober 2025
start_date = pd.to_datetime("01-02-2025", dayfirst=True)
end_date   = pd.to_datetime("31-10-2025", dayfirst=True)

# 4. Filter hanya baris dalam rentang tanggal tersebut
mask = (df['Tgl KA'] >= start_date) & (df['Tgl KA'] <= end_date)
df_filtered = df.loc[mask].copy()

print(f"Jumlah baris setelah difilter: {len(df_filtered)}")

# 5. Simpan sebagai Rekap gangguan 3
path_output = "/content/drive/MyDrive/Rekap gangguan 3.csv"
df_filtered.to_csv(path_output, index=False)

print(f"File berhasil disimpan sebagai: {path_output}")

# 6. Tampilkan 10 contoh data hasil filter
df_filtered.head(10)


import pandas as pd

# Baca file hasil sebelumnya
path_rekap = "/content/drive/MyDrive/Rekap gangguan 4.csv"
df = pd.read_csv(path_rekap)

# Cek jumlah cell kosong
jumlah_kosong = df["Nama_Penyebab"].isna().sum()
print(f"Jumlah cell kosong pada kolom Nama_Penyebab: {jumlah_kosong}")

# Jika ada, tampilkan baris yang kosong
if jumlah_kosong > 0:
    print("\nBaris dengan Nama_Penyebab kosong:")
    display(df[df["Nama_Penyebab"].isna()].head(20))  # tampilkan 20 baris pertama
else:
    print("\nSemua cell pada kolom Nama_Penyebab terisi ✔️")

import pandas as pd

# Baca file terakhir
path_rekap = "/content/drive/MyDrive/Rekap gangguan 4.csv"
df = pd.read_csv(path_rekap)

# Tampilkan nilai unik kolom lokasi
print("=== Nilai unik pada kolom 'Lokasi' ===")
print(df['Lokasi'].unique())
print("\nJumlah nilai unik:", df['Lokasi'].nunique())

print("\n\n=== Nilai unik pada kolom 'Lokasi.1' ===")
print(df['Lokasi.1'].unique())
print("\nJumlah nilai unik:", df['Lokasi.1'].nunique())

import pandas as pd
import numpy as np

# 1. Baca file terakhir
path_rekap = "/content/drive/MyDrive/Rekap gangguan 4.csv"
df = pd.read_csv(path_rekap)

# 2. Deteksi mana yang terisi di kolom lokasi dan lokasi.1
#    Di sini: '-' juga dianggap KOSONG
lokasi_str = df['Lokasi'].astype(str).str.strip()
lokasi1_str = df['Lokasi.1'].astype(str).str.strip()

lokasi_filled = lokasi_str.ne('') & lokasi_str.ne('-') & df['Lokasi'].notna()
lokasi1_filled = lokasi1_str.ne('') & lokasi1_str.ne('-') & df['Lokasi.1'].notna()

# 3. Buat kolom jenis lokasi berdasarkan aturan:
#    - hanya satu yang terisi  -> "stasiun"
#    - dua-duanya terisi      -> "petak jalan"
#    - dua-duanya kosong      -> kosong ("")
df['jenis lokasi'] = np.select(
    [
        lokasi_filled ^ lokasi1_filled,   # XOR: hanya satu yang terisi
        lokasi_filled & lokasi1_filled    # keduanya terisi
    ],
    [
        'stasiun',
        'petak jalan'
    ],
    default=''
)

# 4. Posisikan kolom 'jenis lokasi' di sebelah kanan 'lokasi.1'
cols = list(df.columns)
idx = cols.index('Lokasi.1')
cols.insert(idx + 1, cols.pop(cols.index('jenis lokasi')))
df = df[cols]

# 5. Simpan sebagai file baru
output_path = "/content/drive/MyDrive/Rekap gangguan 5.csv"
df.to_csv(output_path, index=False)

print("Proses selesai ✔️")
print(f"File disimpan sebagai: {output_path}")
print("Contoh 10 baris pertama:")
df.head(10)
