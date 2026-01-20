# ASUS Modem / Router www Dizini Yedekleme (Netcat ile)

Bu rehber, **ASUS (BusyBox / Dropbear tabanlı)** modemlerde  
**SCP / SFTP çalışmadığı durumlarda** `/www` dizinini **netcat (nc)** kullanarak
bilgisayara yedeklemeyi anlatır.

> ⚠️ Bu işlem **sadece size ait cihazlarda** ve **yedekleme / analiz amaçlı** yapılmalıdır.

---

## 🎯 Amaç

- SCP / SFTP yokken dosya transferi yapmak
- Modem arayüz dosyalarını (`/www`) PC’ye almak
- Read-only dosya sistemine yazma problemi yaşamamak
- Ek yazılım kurmadan aktarım yapmak

---

## 🧰 Gereksinimler

### Modem
- ASUS Router / Modem
- BusyBox tabanlı Linux
- SSH erişimi açık

### Bilgisayar (Windows)
- Netcat (nc)
- CMD veya PowerShell
- Aynı yerel ağda olmak

---

## 📌 Ağ Bilgileri (Örnek)

| Cihaz | IP |
|----|----|
| Modem | `192.168.2.1` |
| PC | `192.168.2.42` |

> ❗ Kendi PC IP adresini kullan

---

## 1️⃣ Modemde www dizinini sıkıştırma

Modemde `/www` dizini **salt okunur** olduğu için `/tmp` kullanılır.

```sh
cd /
tar -czf /tmp/www.tar.gz www
```

Kontrol:
```sh
ls -lh /tmp/www.tar.gz
```

---

## 2️⃣ Windows’ta Netcat dinleyici başlatma

CMD veya PowerShell **yönetici olarak** açılır.

```cmd
nc -l -p 9000 > C:\Users\KULLANICI_ADI\Desktop\www.tar.gz
```

📌 Bu komut:
- 9000 portunu dinler
- Gelen veriyi `www.tar.gz` olarak kaydeder

---

## 3️⃣ Modemden PC’ye dosya gönderme

Modemde:

```sh
nc 192.168.2.42 9000 < /tmp/www.tar.gz
```

Aktarım bittiğinde Windows tarafında dosya otomatik oluşur.

---

## 4️⃣ Arşivi açma (Windows)

```cmd
tar -xzf www.tar.gz
```

veya WinRAR / 7-Zip ile açılabilir.

---

## 🧹 İşlem Sonrası Temizlik (Önerilir)

Modemde geçici dosyayı sil:

```sh
rm -f /tmp/www.tar.gz
```

---

## ❓ Sık Sorulan Sorular

### SCP neden çalışmıyor?
- Dropbear `sftp-server` içermez
- ASUS firmware’lerde sık görülür

### Tüm modem dosyalarını tar olmadan çekebilir miyim?
- Hayır ❌
- Netcat **tek yönlü stream** çalışır
- Dizin yapısı için mutlaka `tar` gerekir

### Bu yöntem güvenli mi?
- Yerel ağda ✔️
- İnternete açık port **kULLANILMAMALI**

---

## ⚠️ Yasal Uyarı

Bu rehber:
- Eğitim
- Yedekleme
- Kendi cihazınızı inceleme

amaçlıdır.

❌ Yetkisiz sistemlerde kullanımı **yasadışıdır**.

---


## 📄 Lisans

MIT License
