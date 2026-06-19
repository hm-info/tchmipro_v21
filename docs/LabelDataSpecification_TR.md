# CNC Dosyasından Barkod Yazdırma Veri Spesifikasyonu

**Doküman Versiyonu:** 1.0
**Tarih:** 19.06.2026
**Durum:** Taslak

---

# 1. Amaç

Bu doküman, CNC dosyaları içerisinde yer alan barkod yazdırma verilerinin formatını ve işlenme kurallarını tanımlar.

Barkod yazdırma sistemi, CNC dosyasından parça bilgilerini okuyacak ve belirtilen sıraya göre tüm barkod etiketlerini yazdıracaktır.

---

# 2. Kapsam

Bu spesifikasyon, barkod etiketi gerektiren tüm üretim CNC dosyaları için geçerlidir.

Barkod verileri CNC dosyası içerisinde özel olarak ayrılmış bir bölümde yer almalıdır.

---

# 3. Veri Yapısı

Barkod verileri aşağıdaki başlangıç ve bitiş etiketleri arasında tanımlanacaktır:

```text id="9wltx7"
(LABEL_DATA_START)

...

(LABEL_DATA_END)
```

Barkod yazdırma yazılımı yalnızca bu alan içerisindeki kayıtları işleyecektir.

---

# 4. Kayıt Formatı

Her kayıt aşağıdaki bilgileri içermelidir:

```text id="3z6a4r"
(PartID,PartName,PartLength,Position:1,Part1,1010,1)
```

## Alan Açıklamaları

| Alan       | Açıklama                   |
| ---------- | -------------------------- |
| PartID     | Parçanın benzersiz kimliği |
| PartName   | Parça adı                  |
| PartLength | Parça boyu (mm)            |
| Position   | Parçanın sırası            |

---

# 5. Örnek

```text id="8sq1ne"
(LABEL_DATA_START)

(PartID,PartName,PartLength,Position:1,Frame_Left,1010,1)
(PartID,PartName,PartLength,Position:2,Frame_Right,1010,2)
(PartID,PartName,PartLength,Position:3,Sash_Top,1020,3)
(PartID,PartName,PartLength,Position:4,Sash_Bottom,1020,4)

(LABEL_DATA_END)
```

---

# 6. İşlevsel Gereksinimler

Yazılım aşağıdaki işlemleri gerçekleştirmelidir:

1. CNC dosyasını yüklemek.
2. LABEL_DATA bölümünü bulmak.
3. LABEL_DATA_START ve LABEL_DATA_END arasındaki kayıtları okumak.
4. Her kaydı ayrıştırmak.
5. Her geçerli kayıt için bir barkod etiketi oluşturmak.
6. Etiketleri Position bilgisine göre sıralı şekilde yazdırmak.

---

# 7. Hata Yönetimi

Yazılım aşağıdaki davranışları göstermelidir:

* LABEL_DATA bölümü dışında kalan kayıtları yok saymak.
* Hatalı veya eksik kayıtları loglamak.
* Geçerli kayıtların işlenmesine devam etmek.
* LABEL_DATA bölümü bulunamazsa hata veya uyarı oluşturmak.

---

# 8. Kabul Kriterleri

Aşağıdaki şartlar sağlandığında geliştirme tamamlanmış kabul edilir:

* LABEL_DATA alanındaki tüm kayıtlar doğru şekilde okunur.
* Her geçerli kayıt için bir barkod etiketi oluşturulur.
* Barkodlar doğru sırada yazdırılır.
* Hatalı kayıtlar süreci durdurmaz.
* Eksik LABEL_DATA bölümü için hata veya uyarı üretilir.

---

# 9. Gelecekteki Genişletmeler

İlerleyen sürümlerde aşağıdaki alanlar eklenebilir:

* Profil Adı
* Sipariş Numarası
* Müşteri Adı
* Malzeme Tipi
* Üretim Parti Numarası

Parser tasarımı mümkün olduğunca bu genişletmelere uyumlu olacak şekilde geliştirilmelidir.
