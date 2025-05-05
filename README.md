# Sistem Manajemen Dealer Motor

---
<p align="center">
  <img src="public/images/logo-unsulbar.png" alt="Logo Unsulbar" width="150"/>
</p>

**PIPI**  
D0223307  

**Mata Kuliah Framework Web Based**  
Tahun 2025  

---

## Deskripsi

**Sistem Manajemen Dealer Motor** adalah aplikasi berbasis web yang dirancang untuk mengelola seluruh proses operasional dealer motor — dari pencatatan stok motor, transaksi penjualan, hingga manajemen pelayanan dan karyawan. Sistem ini mendukung tiga jenis role pengguna:

1. **Admin**: Kelola data user, motor, dan transaksi, konfirmasi transaksi, lihat semua data dan laporan.  
2. **Sales**: Input data customer, buat dan kelola transaksi penjualan, lihat stok motor dan riwayat penjualannya.  
3. **Customer**: Lihat katalog motor, ajukan pembelian, lihat status transaksi.

---

## Fitur Utama

1. **Autentikasi Pengguna**  
   Pengguna dapat mendaftar, login, dan logout.

2. **Pengelolaan Produk**  
   Admin dapat menambahkan, mengedit, dan menghapus data motor, serta menyembunyikan produk dari katalog jika stok tidak tersedia.

3. **Pemesanan dan Pembelian Produk**  
   Customer dapat melihat katalog motor, menambah ke keranjang, melakukan pemesanan, dan melacak status pesanan.

4. **Manajemen Role Akses**  
   Sistem mendukung tiga peran pengguna utama: Admin, Customer, dan Sales, masing-masing dengan hak akses yang berbeda.

5. **Pencarian Produk**  
   Customer dapat mencari motor berdasarkan nama, merk, atau kategori.

6. **Laporan Penjualan**  
   Admin dapat melihat riwayat transaksi, ringkasan pendapatan, dan status pengiriman.

---

## Struktur Tabel

### A. Tabel `users`

| Field           | Tipe Data                      | Deskripsi                                 |
|----------------|--------------------------------|-------------------------------------------|
| id             | BIGINT (Auto Increment)        | Primary key, ID unik pengguna             |
| name           | VARCHAR(255)                   | Nama lengkap pengguna                     |
| email          | VARCHAR(255)                   | Email unik untuk login                    |
| phone          | VARCHAR(20)                    | Nomor HP pengguna (opsional)             |
| address        | TEXT                           | Alamat pengguna (opsional)               |
| password       | VARCHAR(255)                   | Password terenkripsi (bcrypt)             |
| role           | ENUM('admin', 'sales', 'customer') | Peran pengguna dalam sistem          |
| remember_token | VARCHAR(100)                   | Token untuk "remember me" saat login     |
| created_at     | TIMESTAMP                      | Waktu data dibuat                         |
| updated_at     | TIMESTAMP                      | Waktu data terakhir diubah                |

### B. Tabel `motorcycles`

| Field       | Tipe Data       | Deskripsi                             |
|-------------|------------------|----------------------------------------|
| id          | BIGINT           | Primary key                            |
| merk        | VARCHAR(100)     | Merek motor                            |
| tipe        | VARCHAR(100)     | Tipe/model motor                       |
| tahun       | YEAR             | Tahun produksi                         |
| warna       | VARCHAR(100)     | Warna motor                            |
| harga       | DECIMAL(12,2)    | Harga motor                            |
| stok        | INTEGER          | Jumlah stok tersedia                   |
| deskripsi   | TEXT             | Deskripsi motor (opsional)            |
| created_at  | TIMESTAMP        | Tanggal dibuat                         |
| updated_at  | TIMESTAMP        | Tanggal diperbarui                     |

### C. Tabel `customers`

| Field        | Tipe Data        | Deskripsi                                 |
|--------------|------------------|--------------------------------------------|
| id           | BIGINT           | Primary key                                |
| user_id      | BIGINT           | Relasi ke tabel `users`                    |
| ktp_number   | VARCHAR(50)      | Nomor KTP                                  |
| pekerjaan    | VARCHAR(100)     | Pekerjaan customer (opsional)              |
| penghasilan  | DECIMAL(12,2)    | Penghasilan bulanan (opsional)             |
| created_at   | TIMESTAMP        | Tanggal dibuat                             |
| updated_at   | TIMESTAMP        | Tanggal diperbarui                         |

### D. Tabel `sales`

| Field        | Tipe Data        | Deskripsi                             |
|--------------|------------------|----------------------------------------|
| id           | BIGINT           | Primary key                            |
| user_id      | BIGINT           | Relasi ke tabel `users`                |
| kode_sales   | VARCHAR(20)      | Kode unik sales                        |
| wilayah      | VARCHAR(100)     | Area penjualan                         |
| created_at   | TIMESTAMP        | Tanggal dibuat                         |
| updated_at   | TIMESTAMP        | Tanggal diperbarui                     |

### E. Tabel `transactions`

| Field         | Tipe Data                        | Deskripsi                          |
|---------------|----------------------------------|-------------------------------------|
| id            | BIGINT                           | Primary key                         |
| customer_id   | BIGINT                           | Relasi ke tabel `customers`         |
| sales_id      | BIGINT                           | Relasi ke tabel `sales`             |
| motorcycle_id | BIGINT                           | Relasi ke tabel `motorcycles`       |
| jumlah        | INTEGER                          | Jumlah unit dibeli                  |
| total_harga   | DECIMAL(12,2)                    | Total harga                         |
| status        | ENUM('pending', 'approved', 'cancelled') | Status transaksi         |
| created_at    | TIMESTAMP                        | Tanggal dibuat                      |
| updated_at    | TIMESTAMP                        | Tanggal diperbarui                  |

### F. Tabel `motorcycle_images`

| Field        | Tipe Data      | Deskripsi                         |
|--------------|----------------|------------------------------------|
| id           | BIGINT         | Primary key                        |
| motorcycle_id| BIGINT         | Relasi ke tabel `motorcycles`      |
| image_path   | VARCHAR(255)   | Path atau nama file gambar         |
| created_at   | TIMESTAMP      | Tanggal dibuat                     |
| updated_at   | TIMESTAMP      | Tanggal diperbarui                 |

---

## Relasi Antar Tabel

1. **users → customers**: One-to-One  
   Setiap pengguna dengan role customer memiliki satu data detail customer.

2. **users → sales**: One-to-One  
   Setiap pengguna dengan role sales memiliki satu data detail sales.

3. **customers → transactions**: One-to-Many  
   Satu customer dapat memiliki banyak transaksi pembelian.

4. **sales → transactions**: One-to-Many  
   Satu sales dapat melayani banyak transaksi.

5. **motorcycles → transactions**: One-to-Many  
   Satu motor bisa terlibat dalam banyak transaksi.

6. **motorcycles → motorcycle_images**: One-to-Many  
   Satu motor dapat memiliki banyak gambar.

7. **users → logs**: One-to-Many  
   Setiap user (customer, admin, kurir) dapat memiliki banyak log aktivitas.

8. **orders → couriers**: Many-to-One  
   Setiap order (pesanan) hanya dapat ditangani oleh satu kurir pada satu waktu.

---
