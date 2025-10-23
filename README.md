# REST API Layanan Cuci Sepatu

Project ini adalah REST API sederhana yang dibuat menggunakan Node.js, Express, dan Supabase untuk mengelola daftar barang (sepatu) pada layanan laundry sepatu.

API ini di-deploy di Vercel.

**Link Deploy Vercel:** https://api-cuci-sepatu-chi.vercel.app/

---

Tujuan & Fitur Utama
Tujuan dari API ini adalah untuk menyediakan backend bagi aplikasi (web atau mobile) layanan cuci sepatu.
Fitur Utama:
* **CRUD:** penuh (Create, Read, Update, Delete) untuk item cucian.
* **Manajemen Status:** Memantau progres cucian (Diterima, Proses, Selesai, Diambil).
* **Filtering:** Mendapatkan daftar item berdasarkan status tertentu (cth: `/items?status=Selesai`).

---

Struktur Data
Data disimpan dalam satu tabel di Supabase bernama `items`.

| Kolom | Tipe Data | Deskripsi |
| :--- | :--- | :--- |
| `id` | `bigint` (PK) | ID unik untuk setiap item (auto-increment). |
| `created_at` | `timestamptz` | Waktu kapan data dibuat. |
| `customer_name`| `text` | Nama pelanggan. |
| `shoe_type` | `text` | Tipe/merek sepatu (opsional). |
| `service_type` | `text` | Jenis layanan (cth: "Deep Clean", "Fast Clean"). |
| `status` | `varchar(50)` | Status pengerjaan. Default: "Diterima". |

---

API Endpoints
Contoh request dan response untuk setiap endpoint.

**Base URL:** `https://api-cuci-sepatu-chi.vercel.app/`

---

1. Menambah Item Baru
Menambahkan data sepatu baru ke dalam antrian. Status otomatis diatur ke "Diterima".

* **Endpoint:** `POST /items`
* **Body (JSON):**
    ```json
    {
      "customer_name": "Budi Setiawan",
      "shoe_type": "Nike Air Jordan",
      "service_type": "Deep Clean"
    }
    ```
* **Response (201 - Created):**
    ```json
    {
      "id": 1,
      "created_at": "2025-10-23T12:00:00.000Z",
      "customer_name": "Budi Setiawan",
      "shoe_type": "Nike Air Jordan",
      "service_type": "Deep Clean",
      "status": "Diterima"
    }
    ```

---

2. Mendapatkan Semua Item (Filter)

Mengambil semua item. Endpoint ini mendukung filter `?status=`.

* **Endpoint:** `GET /items`
* **Contoh (Filter Selesai):** `GET /items?status=Selesai`
* **Contoh (Tanpa Filter):** `GET /items`
* **Response (200 - OK):**
    ```json
    [
      {
        "id": 2,
        "created_at": "2025-10-23T13:00:00.000Z",
        "customer_name": "Siti Aminah",
        "shoe_type": "Adidas Samba",
        "service_type": "Fast Clean",
        "status": "Selesai"
      },
      {
        "id": 1,
        "created_at": "2025-10-23T12:00:00.000Z",
        "customer_name": "Budi Setiawan",
        "shoe_type": "Nike Air Jordan",
        "service_type": "Deep Clean",
        "status": "Proses"
      }
    ]
    ```

---

3. Mendapatkan Item Spesifik

Mengambil satu item berdasarkan `id`.

* **Endpoint:** `GET /items/:id`
* **Contoh:** `GET /items/1`
* **Response (200 - OK):**
    ```json
    {
      "id": 1,
      "created_at": "2025-10-23T12:00:00.000Z",
      "customer_name": "Budi Setiawan",
      "shoe_type": "Nike Air Jordan",
      "service_type": "Deep Clean",
      "status": "Proses"
    }
    ```

---

4. Mengubah Data/Status Item

Mengubah data item, paling sering digunakan untuk update status.

* **Endpoint:** `PUT /items/:id`
* **Contoh:** `PUT /items/1`
* **Body (JSON):**
    ```json
    {
      "status": "Selesai",
      "customer_name": "Budi"
    }
    ```
* **Response (200 - OK):**
    ```json
    {
      "id": 1,
      "created_at": "2025-10-23T12:00:00.000Z",
      "customer_name": "Budi",
      "shoe_type": "Nike Air Jordan",
      "service_type": "Deep Clean",
      "status": "Selesai"
    }
    ```

---

5. Menghapus Item

Menghapus data item (misalnya jika terjadi salah input).

* **Endpoint:** `DELETE /items/:id`
* **Contoh:** `DELETE /items/1`
* **Response (200 - OK):**
    ```json
    {
      "message": "Item berhasil dihapus"
    }
    ```

---

Instalasi & Menjalankan Lokal

Cara menjalankan API ini di komputer lokal:

1.  **Clone Repository:**
    ```bash
    git clone https://github.com/hppys/api-cuci-sepatu.git
    ```

2.  **Install Dependencies:**
    ```bash
    npm install
    ```

3.  **Buat file `.env`:**
    Buat file `.env` di root proyek dan isi dengan kredensial Supabase Anda:
    ```env
    SUPABASE_URL=URL_PROYEK_SUPABASE_ANDA
    SUPABASE_KEY=KUNCI_ANON_PUBLIC_SUPABASE_ANDA
    ```

4.  **Jalankan Server:**
    ```bash
    npm start
    ```

5.  API akan berjalan di `http://localhost:3000`.

**Carlos Abram Sirait - 21120123120030**
