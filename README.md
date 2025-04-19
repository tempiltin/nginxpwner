

# Nginxpwner

<p align="center"><img src="https://i.postimg.cc/vm3LWFj4/nginxpwner.png" /></p>

**Nginxpwner** — bu oddiy vosita bo‘lib, keng tarqalgan **Nginx noto‘g‘ri sozlamalari** va **zaifliklarini** aniqlash uchun ishlatiladi.

## O‘rnatish:

```
cd /opt
git clone https://github.com/stark0de/nginxpwner
cd nginxpwner
chmod +x install.sh
./install.sh
```

## Docker orqali o‘rnatish:
```
git clone https://github.com/stark0de/nginxpwner
cd nginxpwner
sudo docker build -t nginxpwner:latest .
```

Docker tasvirini ishga tushirish:
```
sudo docker run -it nginxpwner:latest /bin/bash
```

## Foydalanish:

```
Burp’dagi Target tab’iga o‘ting, hostni tanlang, sichqonchaning o‘ng tugmasini bosing, “copy all URLs in this host” ni tanlang, faylga saqlang.

cat urllist | unfurl paths | cut -d"/" -f2-3 | sort -u > /tmp/pathlist 

Yoki dasturdan allaqachon aniqlagan path (yo‘llar) ro‘yxatini boshqa bir usulda oling. Eslatma: path’lar / belgisi bilan boshlanmasligi kerak.

Va nihoyat:

python3 nginxpwner.py https://example.com /tmp/pathlist
```

## Izohlar:

Aslida quyidagilarni tekshiradi:

- Nginx versiyasini aniqlaydi va `searchsploit` orqali mavjud eksploitlarni topadi hamda versiya eskirganmi yoki yo‘qmi, shuni bildiradi  
- Nginx uchun maxsus wordlist yordamida **gobuster** orqali fuzzer qiladi  
- `$uri` ni redirect’da noto‘g‘ri ishlatish orqali yuzaga keluvchi **CRLF zaifligini** tekshiradi  
- Berilgan barcha path’larda CRLF zaifligini tekshiradi  
- **PURGE HTTP** metodi tashqi foydalanuvchi uchun ochiqmi, shuni aniqlaydi  
- O‘zgaruvchi (variable) sizib chiqishiga sabab bo‘ladigan noto‘g‘ri sozlamalarni tekshiradi  
- `merge_slashes off` sozlamasi orqali yuzaga keladigan **path traversal** zaifligini tekshiradi  
- Hop-by-hop header (masalan, `X-Forwarded-Host`) lar bilan yuborilgan so‘rovlar javobi uzunligidagi farqlarni tekshiradi  
- **Kyubi** vositasidan foydalanib, noto‘g‘ri `alias` orqali yuzaga keladigan path traversal’ni aniqlaydi  
- `X-Accel-Redirect` orqali 401/403 statusli sahifalarni chetlab o‘tishni sinovdan o‘tkazadi  
- Raw backend javoblarini noto‘g‘ri o‘qish muammosi uchun payloadni ko‘rsatadi  
- Sayt PHP ishlatadimi — tekshiradi va PHP saytlar uchun Nginx’ga xos testlarni tavsiya qiladi  
- Nginx’ning `range` filter modulida mavjud bo‘lgan keng tarqalgan **butun sonlar toshishi (integer overflow)** zaifligini (CVE-2017-7529) tekshiradi

Ushbu vosita javoblardagi `Server` sarlavhasidan foydalanadi. Biroq ba’zi CMS yoki Nginx asosida qurilgan tizimlar (masalan: Centminmod, OpenResty, Pantheon yoki Tengine) bu header’ni yubormaydi. Bunday hollarda `nginx-pwner-no-server-header.py` skriptidan foydalaning (shu bilan bir xil parametrlar bilan).

Ekspluat qidiruvi to‘g‘ri ishlashi uchun vaqti-vaqti bilan Kali Linux’da quyidagini bajaring:

```
searchsploit -u
```

Ushbu vosita **web cache poisoning/deception** yoki **request smuggling** zaifliklarini aniqlamaydi — bularni tekshirish uchun boshqa maxsus vositalardan foydalaning. **NginxPwner** asosan `nginx.conf` faylida ishlab chiquvchi bilmagan holda qilgan **noto‘g‘ri sozlamalarni** aniqlashga qaratilgan.

**Kyubi** vositasi uchun @shibli2700 ga, shuningdek **Gobuster** mualliflariga va **Detectify** jamoasiga (ular NGINX’dagi ko‘plab noto‘g‘ri sozlamalarni aniqlaganlar) rahmat.

---
tugadi!
