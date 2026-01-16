Localden(Window) sanal cihaza(ubuntu) ssh bağlantısı?

1️⃣ Ubuntu içinde SSH Server kurulu mu kontrol et

Ubuntu VM’de terminal aç:
	sudo apt update
	sudo apt install openssh-server -y

Servis çalışıyor mu kontrol et:
	sudo systemctl status ssh

✅ active (running) görmelisin

Eğer çalışmıyorsa:
	sudo systemctl enable ssh
	sudo systemctl start ssh

2️⃣ Ubuntu’nun IP adresini öğren

Ubuntu VM terminalinde:
	ip a
veya daha sade:
	hostname -I
👉 Bu IP’yi not alacağız.


3️⃣ Sanal makine ağ ayarını kontrol et (ÇOK ÖNEMLİ)

VirtualBox kullanıyorsan:

Ayarlar → Network → Attached to

--------------------------------------------------------------

**Seçenek 1: NAT (Port Forwarding gerekir)**

Windows → Ubuntu direkt bağlanamaz

Port yönlendirme gerekir (aşağıda anlattım)

----

🔁 VirtualBox NAT → SSH Port Forwarding (Adım Adım)

1️⃣ Sanal makine KAPALI olsun

Bu çok önemli ⚠️

VM çalışıyorsa kapat.


2️⃣ VirtualBox ayarlarına gir

VirtualBox aç

Ubuntu VM’yi seç

Settings → Network


3️⃣ Adapter 1 ayarları

Şunlar açık olmalı:

✔ Enable Network Adapter

Attached to: NAT

✔ Cable Connected



4️⃣ Port Forwarding ekranı

Advanced butonuna tıkla

Port Forwarding → tıkla

Açılan pencerede sağdaki + (Add New Rule) butonuna bas


5️⃣ SSH kuralını ekle (ÇOK ÖNEMLİ)

Alan	Ne yazılacak

Name	SSH

Protocol	TCP

Host IP	(BOŞ BIRAK)

Host Port	2222

Guest IP	(BOŞ BIRAK)

Guest Port	22



📌 Host IP ve Guest IP boş kalmalı

📌 22 → SSH’in Ubuntu içindeki portu

📌 2222 → Windows’tan bağlanacağın port

👉 OK → OK ile çık

----
4️⃣ Windows’tan SSH ile bağlanma

Windows 10 / 11 → SSH hazır gelir

PowerShell veya CMD aç:
	ssh kullanici\_adi@UBUNTU\_IP


NAT + Port Forwarding kullanıyorsan:
	👉 ASLA VM IP’si yazılmaz
	👉 HER ZAMAN localhost kullanılır

------------------------------------------------------------------

**Seçenek 2: Bridged Adapter**

VM, ağda ayrı bir cihaz gibi görünür

VirtualBox için:

VM kapalıyken

Settings → Network

Adapter 1

Attached to: → Bridged Adapter

Name: → Aktif ağ kartını seç (Wi-Fi veya Ethernet)


VM’yi aç → Ubuntu’da tekrar IP’ye bak:

ip a

Artık şöyle bir IP görmelisin:

192.168.1.45

Windows’tan direkt bağlan:

ssh user@192.168.1.45


✅ Port forwarding gerekmez

✅ Gerçek makine gibi davranır

-----------------------------------------------------------------

**Seçenek 3: Host-only Adapter**


Sadece Windows ↔ Ubuntu arası iletişim

Ayar:

Attached to: → Host-only Adapter


Ubuntu IP’si genelde:

192.168.56.x

Bağlantı:

ssh user@192.168.56.101

⚠️ İnternet erişimi olmaz (tek başına)


--------------------------------------------------------------

