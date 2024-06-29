# **Bigdata Analysis - Kimia Farma**

Visualization : [Lihat dashboard](https://lookerstudio.google.com/reporting/d1c0d12d-e05d-4569-83c4-ae308894e02d)
<br>
Dataset : [PBI kimia farma](https://drive.google.com/drive/folders/1lmHjlOHFhlVC0gpGcmpB5ENeJI51uQms?usp=sharing)
<br>
Tabel Analisa: [Hasil tabel analisa](https://drive.google.com/file/d/1WShCHDzb7H3p5jY89vEHPEX-41umeKfk/view?usp=sharing)
<br>
---
**Introduction**
<br>
Program Project Based Internship kolaborasi [Rakamin Academy](https://www.rakamin.com/) dan [Kimia Farma](https://www.kimiafarma.co.id/). Pada project ini saya berperan sebagai Data Analyst Intern yang diminta untuk menganalisis performa perusahaan menggunakan data-data yang telah disediakan.<br>
<br>
**Objectives**
- Membuat tabel analysis 
- Membuat visualisasi/dashboard Performance Analysis di Kimia Farma pada tahun 2020-2023
<br>

**Dataset** 
<br>
Dataset yang disediakan terdiri dari tabel-tabel berikut:
- [kf_final_transaction.csv](https://drive.google.com/file/d/1OCl_PGpyGkp_I-L8eMTX0BzzdImc0l6m/view?usp=sharing)
- [kf_inventory.csv](https://drive.google.com/file/d/11AkoPyC3x2cVDC1Fe3FQq1ZbuZHrz1Gs/view?usp=sharing)
- [kf_kantor_cabang.csv](https://drive.google.com/file/d/1GfiT2_eTSSGa_OAUvj8IljmoCYgIGkdw/view?usp=sharing)
- [kf_product.csv](https://drive.google.com/file/d/123-x6uvBk58qf8vieQyOcvFNW8J6z0f4/view?usp=sharing)
<br>


---
## Tabel Analisa
<br>
Setelah mengimport dataset ke bigquery, selanjutnya membuat tabel analisa menggunakan syntaks SQL. Tabel Analisa berisi table agregasi dari ke empat tabel yang sudah diimport. Berikut ini adalah kolom kolom yang mandatory pada table tersebut:
<br>
- transaction_id 	: kode id transaksi, 
- date 			: tanggal transaksi dilakukan, 
- branch_id 		: kode id cabang Kimia Farma, 
- branch_name 		: nama cabang Kimia Farma, 
- kota 			: kota cabang Kimia Farma,
- provinsi 		: provinsi cabang Kimia Farma, 
- rating_cabang 	: penilaian konsumen terhadap cabang Kimia Farma
- customer_name 	: Nama customer yang melakukan transaksi
- product_id	 	: kode product obat,
- product_name 		: nama obat,
- actual_price 		: harga obat,
- discount_percentage 	: Persentase diskon yang diberikan pada obat,
- persentase_gross_laba : Persentase laba yang seharusnya diterima dari obat dengan ketentuan berikut:<br>
■ Harga <= Rp 50.000 -> laba 10%<br>
■ Harga > Rp 50.000 - 100.000 -> laba 15% <br>
■ Harga > Rp 100.000 - 300.000 -> laba 20% <br>
■ Harga > Rp 300.000 - 500.000 -> laba 25% <br>
■ Harga > Rp 500.000 -> laba 30%,<br>
- nett_sales	: harga setelah diskon,
- nett_profit 	: keuntungan yang diperoleh Kimia Farma,
- rating_transaksi 	: penilaian konsumen terhadap transaksi yang dilakukan.
 <br>

<details>
  <summary> Klik untuk melihat Query </summary>
<br>
    
```sql
--Membuat Tabel Analisa

CREATE TABLE `rakamin-kf-analytics-427207.kimia_farma.kf_analysis_table` AS(
SELECT
  ft.transaction_id,
  ft.date,
  ft.branch_id,
  kc.branch_name,
  kc.kota,
  kc.provinsi,
  kc.rating rating_cabang,
  ft.customer_name,
  ft.product_id,
  p.product_name,
  p.price actual_price,
  ft.discount_percentage,
--Menghitung Persentase gross laba sesuai ketentuan
  CASE 
    WHEN p.price <= 50000 THEN 0.1
    WHEN p.price > 50000 AND p.price <= 100000 THEN 0.15
    WHEN p.price > 100000 AND p.price <= 300000 THEN 0.2
    WHEN p.price > 300000 AND p.price <= 500000 THEN 0.25
    WHEN p.price > 50000 THEN 0.3
  END persentase_gross_laba,
--Menghitung Nett Sales (Harga Setelah Diskon)
  p.price-(p.price*ft.discount_percentage) nett_sales,
--Menghitung Nett Profit (Keuntungan setelah diskon)
  ft.price-(p.price-(p.price*ft.discount_percentage)) nett_profit,
  ft.rating rating_transaksi
FROM
  `rakamin-kf-analytics-427207.kimia_farma.kf_final_transaction` as ft
--Menggabungkan tabel final transaksi dan tabel kantor cabang
JOIN
  `rakamin-kf-analytics-427207.kimia_farma.kf_kantor_cabang` as kc ON ft.branch_id=kc.branch_id
--Menggabungkan tabel final transaksi dan tabel produk
JOIN
  `rakamin-kf-analytics-427207.kimia_farma.kf_product` as p ON ft.product_id=p.product_id
)

```

<br>
</details>
<br>


---
## **Data Visualization**
<br>
<details>
  <summary> Klik untuk melihat link Dashboard </summary>
	<br>
https://lookerstudio.google.com/reporting/d1c0d12d-e05d-4569-83c4-ae308894e02d

</details>
<br>

---
## Terima Kasih!

Terima kasih telah mengunjungi repositori ini! Jika Anda memiliki pertanyaan atau saran, jangan ragu untuk menghubungi saya.

### Kontak

- **Email**: [almaidah040@gmail.com](mailto:almaidah040@gmail.com)
- **LinkedIn**: [LinkedIn](https://www.linkedin.com/in/al-maidah-/)
