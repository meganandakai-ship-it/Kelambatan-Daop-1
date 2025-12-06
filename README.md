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
