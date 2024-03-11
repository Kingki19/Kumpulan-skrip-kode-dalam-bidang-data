# Kumpulan skrip kode dalam bidang data
- Kalau mau berbagi silahkan wir, jangan pelit. Gw buat ini juga nanti kalau mau kupakai lagi ga cari-cari lagi. Gw udah buat banyak fungsi buat analisis, pembersihan, dan pemodelan sering lupa naruh dimana. 
- Jika ada eror pada suatu fungsi, taruh saja di issues errornya bagian mana.
- Kalau mau share kode silahkan, opsi 1: bikin cabang dan pull request kesini (nanti saya cek dulu), opsi 2: share kode kalian di Issues.
# Daftar Isi
- [Time Series](#time-series)
   - [Exploratory Data Analysis](#exploratory-data-analysis)
      - [Ngecek total baris, total kolom, range tanggal, dan nama semua kolom](#fungsi-untuk-mengecek-total-baris-total-kolom-range-tanggal-dan-nama-nama-kolom-dalam-data-timeseries)
   - [Data Cleaning](#data-cleaning)
      - [Meresample data timeseries](#fungsi-untuk-meresample-data-timeseries)

## Time Series

### Exploratory Data Analysis

#### **Fungsi untuk mengecek total baris, total kolom, range tanggal, dan nama-nama kolom dalam data timeseries**
```python
def print_shape_col(nama_df, df):
  '''
  Memberikan output ukuran dan semua kolom dalam dataframe
  '''
  print(f'{nama_df}:')
  print(f'Total rows: {df.shape[0]}')
  print(f'Total columns: {df.shape[1]}')
  # kolom tanggal jadikan index dulu dan lakukan di luar fungsi ini
  tanggal_awal = df.index.min()
  tanggal_akhir = df.index.max()
  print(f'Range tanggal: {tanggal_awal} sampai {tanggal_akhir}')
  print(f'Columns**: {df.columns}')
```



### Data Cleaning

#### **Fungsi untuk meresample data timeseries**  
  Contoh penggunaan: misal jika urutan tanggal data timeseries-mu ada tanggal yang hilang atau kosong. Fungsi ini dapat menyelesaikan masalah itu.  
  Referensi parameter `.resample` ada [disini](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.resample.html)
  ```python
  def fill_date_resampling(df, kolom_tanggal, kolom_exclude=[], format_tanggal='%d-%m-%Y', frequensi_resampling='D'):
    '''
    params:
    df = dataframe yang digunakan, pastikan data sudah dalam bentuk dataframe. 
      gunakan metode pd.read dari library pandas
    kolom_tanggal = kolom tanggal yang mau diresampling
    kolom_exclude = list kolom yang tidak ingin diisi dengan interpolasi
    format_tanggal = format datetime pada kolom tanggal pada data
      default = '%d-%m-%Y'. Contoh: 01-02-2017
    frequensi_sampling = frekuensi datetime pada data. Misalnya kalau data harian, bulanan, atau tahunan.
      default = 'D' yang berarti harian.
  
    returns:
    mengembalikan dataframe yang sudah diresample dan diisi nilai yang hilang menggunakan interpolasi
    '''
  
    # Ubah format tanggal menjadi tipe datetime
    df[kolom_tanggal] = pd.to_datetime(df[kolom_tanggal], format=format_tanggal)
  
    # Set kolom_tanggal sebagai index
    df.set_index(kolom_tanggal, inplace=True)
  
    # Resample data harian menjadi data harian lengkap
    df_resampled = df.resample(frequensi_resampling).asfreq()
  
    # Isi nilai yang hilang dengan metode interpolasi
    for kolom in df_resampled.columns:
      if kolom not in kolom_exclude:
        df_resampled[kolom] = df_resampled[kolom].interpolate()
  
    # Reset index untuk mendapatkan kolom 'date' kembali
    df_resampled.reset_index(inplace=True)
  
    # Return DataFrame yang diresample
    return df_resampled

  ```

# Daftar Isi
1. [Header 1](#header-1)
   - [Subheader 1.1](#subheader-11)
   - [Subheader 1.2](#subheader-12)
2. [Header 2](#header-2)
   - [Subheader 2.1](#subheader-21)
   - [Subheader 2.2](#subheader-22)

## Header 1
### Subheader 1.1
Isi subheader 1.1

### Subheader 1.2
Isi subheader 1.2

## Header 2
### Subheader 2.1
Isi subheader 2.1

### Subheader 2.2
Isi subheader 2.2
