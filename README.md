# Kumpulan skrip kode dalam bidang data
Kalau mau berbagi silahkan wir, jangan pelit. Gw buat ini juga nanti kalau mau kupakai lagi ga cari-cari lagi.
## Timeseries
### **Fungsi untuk meresample data timeseries**  
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
