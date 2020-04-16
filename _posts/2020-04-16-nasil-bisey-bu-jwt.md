---
title: Nasıl Bişey Bu JWT
layout: post
date: '2020-04-16 04:14:00'
categories:
- jwt
---

# `JWT`
Açılımı JSON Web Token, kendileri veri alışverişi yaparken  doğrulamadan sorumlu.
3 kısımdan oluşuyor.
* HEADER 
* PAYLOAD 
* SIGNATURE

# `Header`
2 keyword var alg ve typ 
alg: Şifrelenme algoritmasını belirtiyor
typ: Token tipi

# `Payload `
Veri barındırdığımız kısım.
Bu kısmındaki veriler başkaları tarafından okunabilir dikkatli olunmalı
ıd pw gibi şeyler burada tutulmamalı yani önemli verileri tutmamak gerek.

Diyelim ki elemanımız payload ı decode etti ve verileri değiştirip yolladı.
Bu payload kısmında bulunan verilerin biri değiştiğinde token değişecektir ve sistem
bu bozulmayı fark eder, tokenı yutmayıp tükürür. Bozulmayı farketmesindeki en önemli etken
`SIGNATURE` kısmında kendi verdiğimiz özel stringi(secret key) kötü niyetli kişinin bilmiyor olmasıdır.
<br />




**`HEADER ve PAYLOAD BASE64 ile kodlanıyor.`**

# `Signature`
HEADER ve PAYLOAD alanlarını şifrelediğimiz kısım.
Yapı olarak ;

**HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  zombiepr0cess666
)**
<br/> 

sonda bulunan  **`zombiepr0cess666`**  benim secret key değerimdir.
Bu secret keyi kendinize göre değiştirip tokeni imzalıyorsunuz bu sizin bir nevi mührünüz
<br />
Token 2 tür olarak karşımıza çıkar.
**Access Token** = Id pw yolladıktan sonra döndüğümüz token endpointlere eriştiğimiz token yanii
**Refresh Token** = Access token ömrünü doldurduktan sonra userdan tekrardan login bilgisi istemeyip arkaplanda tekrardan bir access token almaya yarıyor
<br />
**`TOKEN DİYE BAHSETTİĞİMİZ ŞEY ACCESS TOKEN `**
