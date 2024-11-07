# Latar belakang

-   AI memerlukan pengetahuan yang spesifik bagi suatu bidang untuk menjawab pertanyaan terkait bidang tersebut
	- Misalnya: sebuah aplikasi bantuan *chat* untuk keuangan harus memahami tren pasar dan produk yang ditawarkan oleh bank tertentu.
- Solusi umum: RAG (*Retrieval-Augmented Generation*)
	- Mengambil data relevan dari knowledge base
	- Menggabungkannya dengan permintaan pengguna
	- Meningkatkan kualitas model
- Pengetahuan bisnis sering terdapat dalam format seperti PDF, presentasi PowerPoint, atau *scan document*
	- Sulit untuk mengekstrak dan menyiapkan bagian-bagian yang relevan untuk diinjeksikan ke dalam *prompt* yang dapat dikirim ke model LLM

# Ringkasan RAG Dasar

- RAG: model mengakses dan menggunakan pengetahuan eksternal dalam jumlah besar.  
- Memproses *knowledge base* besar dan secara dinamis mengambil informasi relevan pada saat *runtime*
 
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcDYStWOyh7nBv3Jv2ZwMID5nLeQyoplBntIE7fxdWaVWg7ujtsSu2YVSrv4Eydw-KidBxXJfA76tz79KF8QiJ8_QwyQbVGO-lpUci4zgrq8Q3yPnmlY8Ft4OBJsJRGAxARGorFnhNkbHU21mTXceEGJ8jZ?key=Y2W_a-IgBVeXx2iyoST-Nw)

# *Efficient Document Retrieval* Menggunakan *Vision Language Models*

- ColPali: sebuah pendekatan baru untuk *image retrieval* yang menghemat proses document retrieval dengan *vision language models*.
	- *Direct indexing* & *embedding* dari *document pages* sebagai gambar-gambar (kotak merah muda)
	- *Retrieval* berdasarkan *visual semantic similarity* (kotak hijau)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdt5L51DjcOw2MLMTAN0_zFLyjufTfZqUc8omxdVOg8YkXrOpsslOGjfsmXHJjI-PXxcAfFBNUtj8KXtJZlREelienTpRmQbUbr8OmvNtSPUWtVu_m27yc-hPGjqaq54foJxg12uiG0l82rbII6b0f_qWsl?key=Y2W_a-IgBVeXx2iyoST-Nw)

# Cara Kerja ColPali:

- Indeks knowledge base:
	- Encoder membagi setiap gambar menjadi *patches* dan menyimpan informasi tekstual dan visual sebagai vektor
	- Vektor-vektor ini dapat disimpan dengan efisien dalam basis data vektor untuk retrieval cepat.
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfBOomRmFZ6EOD4lHhPPdrJ0_xKyzo4UMqpqwjUVEAnMt090L571jLpSTVvQxZP2hCFQaTNDYoMAziOQu9jiAqdMnAd8Qzu-laU_ZWEw-4IS31JNea61eFyMKM-Un6GYOKnvENinfibKhFChpfGR7EH0pen?key=Y2W_a-IgBVeXx2iyoST-Nw)

- Query Processing & Retrieval:
	- Ketika pengguna memasukkan prompt, prompt tersebut diproses dan query diekstrak
	- Sistem kemudian mencari dalam basis data vektor untuk potongan-potongan teks yang semantiknya sama dengan query
	- Interaksi antara vision tokens dengan language tokens ini memungkinkan interaksi semantik yang sangat kaya antara query dan dokumen untuk mendapatkan kesamaan.
	- Proses ini menghasilkan retrieval dan ranking halaman dokumen yang paling relevan terhadap permintaan
	- ColPali dapat memproduksi heatmap semantik (menampilkan bagian dokumen yang paling dekat dengan query), sehingga memberikan pengguna wawasan intuitif tentang proses retrieval.
	- Potongan-potongan yang paling relevan diambil dan disematkan ke dalam prompt yang dikirim ke model generatif AI

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdMAmeyg1kj9R1zaPcDDZQvp9cs2nILX5yyVSojqJkMbHyeODwRKKPns66XNPfHqWfNIXYSYJcFYL99Zn_b-33qkAa4RtQwiXE9ReUdZCFGF755RXHNrg6UNdqLXi1xgMv-Hqpi6DuXWe9IJvqqibqXtL5B?key=Y2W_a-IgBVeXx2iyoST-Nw)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd5rtVKM7TJOe6VjshgDqAJS0j81CGw_u3Tq9Yf3pV7p8TmiaH9p-RmfutbXXqmgM4kM0ZxG7uD4XbvoU64y-_Rf59DznFas9rgGf_koYninXPpmtz4zAKY8v8iLyzeA59uJjMW1PL4Yqt5VeQgE_cV_1Q6?key=Y2W_a-IgBVeXx2iyoST-Nw)
- *Response Generation*:
	- Model AI kemudian menggunakan informasi yang diperoleh dan pengetahuan *pre-trained* untuk menghasilkan respons
	- Konteks yang diberikan mengurangi halusinasi dan kita dapat memberi tahu sumber dari jawaban kita

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfstS5zOyCkEpY832CHYblbjrnjmt5_57-Oqnw6B7Q1PdVq6L2s2hE-e46S11uqlgAOwSQ1vIAz8LH0AFFaSqLUJHgSKMEwZS7rMxMDLeNxg5phKfYjQodF1WrDy1UnYWqfvkTzNaB_jIjVL-X0WhFLy5o?key=Y2W_a-IgBVeXx2iyoST-Nw)

- ColPali menganggap semua dokumen menjadi gambar
	- Setiap dokumen diperlakukan sama tanpa perlu penanganan spesifik
	- Pendekatan ini menyimpan layout asli dokumen (ini sangat penting dalam menjaga konteks dan arti) terutama pada dokumen yang kaya visual -> pemahaman menyeluruh dari isi dokumen  
	- Kemampuan ini sangat berguna saat menghadapi dokumen yang kombinasi teks, grafik, diagram, dan data visual lainnya.

# Kekurangan ColPali

- Kelemahan ColPali: harus berhadapan dengan lebih banyak vektor dibandingkan dengan metode konvensional. 
- Salah satu cara mengatasi: DSE (Document Screenshot Embedding)  
- DSE menggunakan pendekatan bi-encoder untuk image retrieval, di mana semua vektor *patch* gambar disederhanakan ke dalam satu vektor yang sama dengan *query*
- Menggunakan metrik jarak seperti kosinus atau euclidean -> Kesamaan antara vektor gambar dan vektor *query*
- Kekurangannya adalah vektornya tidak semantik kaya seperti pendekatan multi-vektor per halaman dokumen.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXddE5c27dmAfi5z2sVuwouY0Csr6tG_XvYNQ1eJu9hElfzzizyBBN-ka5GiwGVUhIy3cuONJHWu-zT3NOkGDf0umLlv_hVFWsz62yVlZPWM9j6bounTSfBIV2omket-3GGl-ImY4u5lYY3LLXU0ZR3uwvPN?key=Y2W_a-IgBVeXx2iyoST-Nw)

# Menggunakan Llama 3.2 Vision untuk Pemahaman Gambar

-   ColPali me-*retrieve* dan me-*ranking* halaman dokumen yang relevan berdasarkan *query*
- ColPali
	- Bisa tahu gambar/halaman mana jawaban/konten relevan berada  
	- Tidak akan menghasilkan jawaban dari pertanyaan tersebut.
- Seri baru Llama 3.2 vision menggunakan teknik disebut instruksi *visual tuning*
	- model bahasa bisa “melihat” dan memproses gambar
	- Kemampuan *vision* LLM didapat dengan menerapkan token-token gambar ke dalam ruang-ruang laten yang sama seperti token-token teks dan melatih untuk menyatukannya.  
- *Efficiency retrieval* ColPali + Llama 3.2
	- bisa menemukan halaman/gambar yang benar
	- memahami dan menjawab pertanyaan tentang konten-konten mereka

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcqJ6PLhbZRIMq2JWqKM4fRX2ygLRQaqWBSvBoBnEpqFLDlenWk4T57MGYFfnIwsk-ztnVy_s15NPVLQFX3uwJAykC1a32_Cw8CCShJ7fcgwuT4mYZmxQPPVjfXe8_PIDLCQLUzpIckW-mfU82IdCj_82Q?key=Y2W_a-IgBVeXx2iyoST-Nw)

- Setelah ColPali mengidentifikasi halaman relevan teratas untuk permintaan tertentu, kita bisa melewatkan halaman-halaman tersebut bersamaan dengan *prompt* ke Llama 3.2 untuk *generation*.
 
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdPFPxLHclkJ44WzO8LD-xH5Xz1wYcj_Ikofo7rbOCl7Nn0PYvrvoQaAqB3b9xSyn0FLIBv7cNM6gtupUkCjhTRitxO2UyWn-7VapCQ1_vXB3-pDOxuq1HrEjh3gVS8eGfMLiARlv2oxyBEQ7I7jkMUwDXx?key=Y2W_a-IgBVeXx2iyoST-Nw)

# Login colab

-   colab.research.google.com

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcy2q00nAqztGCOFxG2YIrsJcA_kYpfxg11Ern6IxuzyHz518dN2SgBQ9SuZxXvPvncrKts8aGOBVM-IcUjKS3cVYR4l2Z-d_VkSqWwh-kU2cPB5MbNwmPJHXLQfzdhgDKLwt9DAf-jpw2CFwtPPl9bgHI?key=Y2W_a-IgBVeXx2iyoST-Nw)

-   Tekan tombol sign in

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcOQ8eeXyx-X3xPmBFinhu-XMMd8NoQolsgzB8UglrXv3R0xx7Sk1wzib-4XDAQmKO0NLXKmViDvigdlM2b3i3kl65VsyGkS1iW-6e_iw7ulNYeoLvtHqeIxCaDMLkUKksoV9NLIY0MYDFLuPQDobl2jN8?key=Y2W_a-IgBVeXx2iyoST-Nw)

-   Masukkan surel

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdI7A3GQO-KADIMzQ1zzza3JmXwbWGdJfYAnBIXRgFmxe6iUFKRoSkhtfpS_RXFPBPSQgjuSBwBb69lRrWt69oh4qCfbMRRuuTKHl1VHXxrXflxKrM8ouOTFfWXlc-yT-oNfptjrKPWLSSFBNmFqQmXFdRr?key=Y2W_a-IgBVeXx2iyoST-Nw)

- Masukkan kata sandi

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcuamloYJRkE94_4BXYKmiCe_sgHjXPtRCUQf8GQDwL6mX9PDSsZuruxUvc5Q4fBW-DiKwFvrV0cg-I7XWrY6x9QI6Z7BVkMqDqsZKU_VzzqQMq0ravU9AGlYYNI9WQUtN5ruyHuE6gx5LL8f2KtMxJtHk?key=Y2W_a-IgBVeXx2iyoST-Nw)

- Berhasil

# Together API Key

[https://api.together.xyz/settings/api-keys](https://api.together.xyz/settings/api-keys)

Gunakan link tersebut untuk mendapatkan kunci

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfuIMfVAsuHCPw7r1ZKx4FCRwJhGYE-ZhZXO301wDWDxhvGyGN27yuHobztuUsBoyYdIdTX7rojcUb6J8r3DSjpjjyeeTc1jxsG2bcCDJX9jY4MBWdAXc7iLgHcvD6xu9z9tDv8DMrTEVorxyND5SAqSH00?key=Y2W_a-IgBVeXx2iyoST-Nw)

Sign in dengan akun kalian

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcUmw6ve9qdl86vN870hdIyNs-fiCWtR7-VI0qv8EgVA85Z_INZJIrFcZsEsVeHHr11CfldFfS-YVlinXdrZNyZUPMCgdqK-dATN6oWVc65_KDFjPeo4MVpK-il_8CzAmiH6yoHgUZqOWDNIgbidrzRFRQ?key=Y2W_a-IgBVeXx2iyoST-Nw)

Salin API key yang didapat

# Cara gunakan berkas colabnya

[Link github](https://github.com/togethercomputer/together-cookbook/blob/main/MultiModal_RAG_with_Nvidia_Investor_Slide_Deck.ipynb)
[Link colab](https://colab.research.google.com/github/togethercomputer/together-cookbook/blob/main/MultiModal_RAG_with_Nvidia_Investor_Slide_Deck.ipynb)

## Connect

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdISYTA-06cvackIMFyWSTx58HqElKrCH3XOBWdcmgm1_McewL-DdgPGyT86irRFDa_n5b2zsqlsOv-Bb3iKq7iUYSLACplohr0c0H45rqs-djW08k_CeVB0zFCJsp8CINyN42PjA?key=Y2W_a-IgBVeXx2iyoST-Nw)

Untuk bisa menjalankan ipynb ini, kita harus menekan tombol connect yang ada di kanan atas sampai menjadi seperti ini

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfsvmmzQvMEGreeOQLkZwDHTtcdmbPfpSAY6IOietKJ2MvVFGrdXQnxih_h4g3U-MAy4kvJqi4zw7Tl6PkwaWfflHUWwWRcVeqEd2QiHbMLH4YgHV4pzuEoJG4tF0D4PbqqqmQM?key=Y2W_a-IgBVeXx2iyoST-Nw)  

## Cara menjalankan cell2nya

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeS3tbdFb8Jxvayi8xeHtMxBeEYsre_yNRMDJTcvyb6ivEh63yIXbBLdvzK4-ObsKxNsnb7gOpZBTBttsz0JfKywXDKAXFpXTpa2Scaf4t8l2oj6G2QQPYxLeCaHy06Y21FFAWxKg?key=Y2W_a-IgBVeXx2iyoST-Nw)

Lakukan salah satu hal ini:
-  Pencet tombol play di kiri 
- Ctrl+enter
 
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc3k3_OREHfhYt_vkcrXSAvK-xL4QjJfWjMjroJZjJmrP08ocTy-PPvdnkg7GP4lkYd50iZ0fYIdnTAuggfIsZtVqJSjXHMUXxRK_6acxjNUgAKz2Le2ndUYeDdpoq77Owl9WUV1w?key=Y2W_a-IgBVeXx2iyoST-Nw)

Jika muncul peringatan ini, “run anyway”

## Unduh pustaka yang dibutuhkan

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd7QNpDwBgUBW6ePo4MFAMD-qWy6H91krrC8f1GxGCF1IDtVtooYBUhL-rcm6XjmGJ681DzToWkfQ9PnQ9rkxqLRHNgrTEpBzeMzrkLwmU2GNWq7xXgepB9AXz-HLRRMPzRnmQxds-33pCzO6GnrE6z1Pi3?key=Y2W_a-IgBVeXx2iyoST-Nw)

Ganti isi variabel api_key dengan api key yang sudah ada di clipboard kalian

Contoh:

api_key = “aaaaaaaaaaaaaaaa”

  

## Inisialisasi Model ColPali

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdJ8Tjr6Hu2cANyOsu2adxRuVvFcrreLVnHCB9tedgvP-FbJ2xOF-pntJ420tXCQvO4pcjWDevHJlu-I6niwMd5bnvNop72l8ucv9vtW1PajgCOh46q-FF4f1E7YlLrXbWAehy9HY5aElTGIGnjGueTVVU?key=Y2W_a-IgBVeXx2iyoST-Nw)

  

## Dapatkan contoh dokumen untuk percobaan

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfH81K6f_76ZX-Q05pdNdsLscZS-b4PL4owNIuhvy61N8Dsx2MEKxG9jiCGPMBDWDv6liV7P993K2ocO8TfpaLA31ZfqtCLgKYQeF8fFF60hNz7OKWlV2Mn317TaU0dsBQbBE99RK9d27tRyMN8eyxMrDkQ?key=Y2W_a-IgBVeXx2iyoST-Nw)

  

## Buat indeks dari gambar-gambar

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcYjHtxZ-S6GREeQODYt4GMKCv_CMHYfqYCePhMNWao34gNhTeU5LrT8kBusU_2wy9ANFBsl0grHMs0U49Wjfiy3DG2G4t_HtTZQ_M8PsrM5GPWALMtH7Hs1sDk3XH1ldw9ttopk5ObiVk4DUkjSG82cGa0?key=Y2W_a-IgBVeXx2iyoST-Nw)

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdDlyLGO5c1Br0fXXFxUe5mcOEzF-J4M36kkqPh6UzGOq_GBQ_-TVPoLS-6n2PnbCruCwYWYckEKOqPD6Y5XuOj4Y7Cm862JRIXY_HxvYmZgbjmLD5DWaEOgkI7HaGJgGOW06Wl4BOLCcut54RqYbMvBF7_?key=Y2W_a-IgBVeXx2iyoST-Nw)

  

## Query dari gambar-gambar yang telah disimpan

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe13nuW0CWJLHSwF6CUDR1OUKCAD72jMSmXRBfkjgvWAhS49kjeS8b86woD-rUbQy7myLk4Re_mAtRYvnRKbJLqKtnY2C0dmmaxkE5putRHJhfNwSTm2dPLmRCZGeW9T8xSHbBiJ5mUUjMnkrVPe_enP_-m?key=Y2W_a-IgBVeXx2iyoST-Nw)

  

Gambar yang tepat:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeR3VupUTCbxUamUNmz6f8ortnQ0_piSspLJaIa4jAgr0LF2GIQMULHJ5q8ub8nBBesrH7ohzXfkaBQH2Y5IV8DjSlmjR0LBxl-SUvmXtKRBy96IlyCJkLq_iQ5ma9b5Lo3RV9q3TPSiKTz58BFrFt8206X?key=Y2W_a-IgBVeXx2iyoST-Nw)

Interaksi antara gambar dan query text untuk memberikan nilai pada setiap halaman

Gambar diresize ke 16x16

Lakukan MaxSim

## Gambar prediksi

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfwCtAIALpV4dkPBF3E6o5e0PLVTerBIXNG-3C-ACmMjokkDVUbHMfMbEQcgfonSBIWKFweEH4mb_IrLA5ykmU8TJHIC1-9bqbkxQSUPr8PvOrv9cE44I-HDKc1uZAjcAWvJCe_mtbcdw93lxngCw_QLo3a?key=Y2W_a-IgBVeXx2iyoST-Nw)

  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdekn3vadQTi-GDb8QjH1U5cuciFnBVeZyPRHp9FMGjc3WuBEEKRTBMo3JzCA7Kl0yUMxAn4akBFNciv7103_Szh--Nas_ycHDfhvAMPpexSg6rAV6XP8BKUvQdWE6lX6z3qtLOc9Uzrp8SQ_G1Z-wqJHqz?key=Y2W_a-IgBVeXx2iyoST-Nw)

## Llama 3.2 dapatkan jawaban dari retrieved_page

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd4Zfn5buDEqOjpJj1nAiwZhOTyQzQdVRBQ01OixQKilF-hrw8qNR-3rQG8UR9tGEq3GUDh9vKVfMjj7vngN2eIcXM-24459Vj8c_p1rfU-IGwMC2QuTHuJh7oC0cacHI3B9ogc-pQAmduYGJ-6GEtYPaMA?key=Y2W_a-IgBVeXx2iyoST-Nw)

[Referensi](https://www.together.ai/blog/multimodal-document-rag-with-llama-3-2-vision-and-colqwen2)
