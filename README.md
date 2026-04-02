## Dokumentasi Cara Kirim Image Study Radiologi Satu Sehat User SIMRS KHANZA

- Sebelum itu harus  sudah terinstall Orthanc (PACS) di Server RS -> Sudah setting Mesin DR ke Orthanc (PACS), Kalau sudah langsung ke Step Untuk kirim Imaging Study Radiologi.

# 1. Download Terlebih Dahulul dicom-router dari KEMENKSES : 
Klik ini...
[Download](https://drive.google.com/drive/folders/1hdSgD8pqj26jgPcPZy4jbrdx_7fU8Lq8?usp=drive_link)


- Setelah download, setting di file router.conf

<img width="777" height="425" alt="Image" src="https://github.com/user-attachments/assets/85a8e99b-070f-44df-918e-a4da0213c8b8" />

- Ubah bagian dibawah ini sesuai dengan data rs masing-masing.
```
organization_id = disesuaikan
client_key = disesuaikan
secret_key = disesuaikan
```
- Setelah selesai setting dicom routernya, lanjut ke step selanjutnya

# 2. Setting orthanc.json
- Cari file di filesistem/etc/orthanc/orthanc.json (Sesuaikan folder di server kalian directorinya)
- Setelah itu ubah baian  ini :
```
"DicomModalities" : {

"SATUSEHAT" : [ "DCMROUTER", "10.0.0.188", 11112 ] -> (10.0.0.188) ini sesuaikan ip komputer yang sudah ada dicom-router yang sudah disetting, (11112) ini port default gkpp pakai ini aja
```
Gambar file orthanc.json :
<img width="913" height="455" alt="Image" src="https://github.com/user-attachments/assets/8358331e-cff3-4cc8-897b-764df0de77a4" />

- Setalah sudah setting file orthanc.json ini, lanjut ke step selanjutnya.

# 3. Lanjut ke dicomerouter. Run file main.exe. Aplikasi ini stable di OS Windows.
Tes apakah berhasil atau tidak, kalau tidak berhasil biasanya langsung force close terminal.exe nya
Gambar Log dibawah ini berhasil terhubug : 
<img width="1888" height="568" alt="Image" src="https://github.com/user-attachments/assets/66998bf2-a71f-4da9-a0d4-82b05ce0e7fc" />

# 4. Sebelum kirim imaging study Radiologi, kirim service procedur Radiologi Satu Sehat di SIMRS Khanza
- lihat log di kotak hitam (cmd) khanzanya, copy log tersebut ke platform AI yang biasa digunakan, seperti claude.ai atau chatgpt, biar si AI yang analisis Accession Number (ACSN)
- Accession Number (ACSN)  adalah nomor unik yang diberikan secara otomatis oleh sistem informasi radiologi (RIS/PACS) untuk setiap pemeriksaan radiologi yang dilakukan pada pasien.
- Fungsi: Nomor ini digunakan untuk melacak pesanan radiologi dan mencocokkan hasil pencitraan (gambar DICOM) dengan permintaan pemeriksaan (service request) di SATUSEHAT.
Penggunaan: Dalam struktur FHIR SATUSEHAT, ACSN digunakan untuk mengidentifikasi studi pencitraan (ImagingStudy)
- Gambar ini hasil analisis si AI 
![Image](https://github.com/user-attachments/assets/334d8e80-7ed9-459a-becd-043d05d4c587)

# 5. Setelah mengetahui ACSN yang terkirim ke kemenkes Kirim file dicome ke orthanc dari mesin DR 
- Setelah dikirim lihat di web orthanc, biasanya dengan doamain http://10.0.0.102:8042/app/explorer.html (10.0.0.102 ip ini tergantung di rs masin2)
  <img width="1000" height="1000" alt="Image" src="https://github.com/user-attachments/assets/e7500ad7-32ac-440c-a85c-b9e3f9d0bb57" />

- Klik all study, lalu pilih pasien yang sudah dikirim ke satu sehat melalui menu SIMRS KHANZA di Service Procedur RADIOLOGI Satu Sehat
- Pilih pasien yang sudah terikirim dari mesin DR
- Klik kirim dicom modality ke dicomrouter yang tadi kita sudah setting
<img width="591" height="301" alt="Image" src="https://github.com/user-attachments/assets/c4d99cbf-0133-4620-8875-efe3d9aa37df" /><img width="696" height="210" alt="Image" src="https://github.com/user-attachments/assets/3a9035ba-748d-4c0c-8aac-d52a11612d0d" />

- Kalau berhasil muncul pilihan dicomrouter seperti gambar diatas
- Setalah itu kirim MODALITY IMAGE ke dicomrouter satu sehat kemenkes
  <img width="1896" height="595" alt="Image" src="https://github.com/user-attachments/assets/0e966486-45bf-4285-be51-0eee142a52de" />
- Kalau sukses ada info succes di web orhtanc nya, Gambar dibawah ini sudah berhasil terkirim ke satu sehat
  ![Image](https://github.com/user-attachments/assets/e05c8fab-a3fb-46ef-89fc-b0cb49d39630)
- Selesai....

## KESIMPULAN DAN SARAN
  1. Dicomerouter terInstall di komputer os windows
  2. Setting dicomerouter di file router.conf
  3. Setting file orthanc.json
  4. Sebelum kirim imaging study ke satu sehat Kirim Terlebih dahulu dari SIMRS KHANZA di Menu SERVICE REQUEST RADIOLOGI SATU SEHAT agar terikirim ACSN nya ke satu sehat
  5. Jika gagal terikirim ke satu sehat. Lihat log setelah kirim data pasien dari Menu SERVICE REQUEST RADIOLOGI SATU SEHAT copy semua log tersebut Tanya ke AI seperti Chatgpt agar bisa menganalisi ACSN yang terkirim ke Satu Sehat itu apa. INI PENTING BGT kalau ACSN Terkirim ke satu sehat MELEBIHI 16 DIGIT akan gagal ketika mengirim dari DICOM-ROUTERNYA

     - CASE Yang diamalami saya pas gagal kirim itu karena melebi 16 digit ACSN saat kirim ke satu sehat
     - dikhanza itu diambil dari NO ORDER ATAU NOMER PERMINTAAN RADIOLGI + dengan KODE PERIKSA radiologi. Diuasahakan menggunakan Angka KODE PERIKSANYA.
<img width="243" height="238" alt="Image" src="https://github.com/user-attachments/assets/391fbf72-9702-4172-bcb0-532c17d2be0a" /> <img width="1000" height="182" alt="Image" src="https://github.com/user-attachments/assets/d3336e55-6b04-48e0-96b6-588095cc03ff" />
    - Kalau di rs kaliam masing panjang KODE PERIKSA nya Harus diganti terlebih dahulu
    - Contoh seperti gambar dibawah ini, ACSNya ketika kirim dari dicomerouter ke satu sehat berbeda, KARENA saat kirim SERVICE REQUEST RADIOLOGINYA berbeda dan melebih 16 DIGIT ![Image](https://github.com/user-attachments/assets/44665c09-569b-404b-ae05-8fc4cf3c033a)
    -  Jadinya saya KALAU SEPERTI ITU fix dari Kirim ulang dengan  SERVICE REQUEST RADIOLOGINYA dan copy lognya lagi tanyakan ke AI kalau sudah diketahui ACSNYA,
    -  Ubah ACSNYA di mesin DR kalian, ikuti ACSN yang sudah dikirim
       
