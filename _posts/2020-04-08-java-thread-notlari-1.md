---
title: 'Java Thread Notları #1'
layout: post
date: '2020-04-08 18:42:06 +0300'
categories: java
---

Java da thread kullanmak için` java.lang.Thread` sınıfını veya `java.lang.Runnable `arayüzünü kullanmamız gerekir.
`java.lang.Thread` sınıfından türetilen sınıflar run()  metodunu override etmeli.
`java.lang.Thread` sınıfından türeyerek oluşturulan her nesnenin` run() `metodunu çağırmak için `start() `metodunu çağırmalıyız.
Her `start()` metodunun çağrılmasıyla thread ready durumuna geçer ready durumunda olması threadin `run()` yordamını çalıştırdığı demek değildir.
Ready durumu bütün threadlerin bekletildiği yerdir. Fakat her thread aynı statüde beklemez yani bunların bir öncelik sırası vardır. Rütbelere sahip olduğunu düşünün aynı rütbe de olanlar aynı yerde tutulur.

öncelik sırası aralığı 1 ila 10
{% highlight java %}
Thread.MIN_PRIORITY =1
Thread.NORM_PRIORITY =5
Thread.MAX_PRIORITY =10
{% endhighlight %}
Şimdi bir thread ilk oluştuğunda normal bir önceliğe sahip olur bunun öncelik ayarlanması `setPriority()` metodu ile yapılabilir,`getPriority()` ile de öncelik sırasını öğrenebiliriz.

bir threadi sonlandırmak demek run metodundan çıkması demektir. Bunu `return` ile yapabiliriz.

**`sleep() `**
Ee adından da tahim etmişsinizdir bu threadi bir süre uyutuyor bu metod statikdir ekstra birşeyler üretmeniz gerekmez direkt çağırılabilir. Mışıl mışıl uykuda olan threadi `interrupt() `metodu ile kalk la hadi diyerekten uyandırabiliriz.

Bir threadin uyurken rahatsız edilip edilmediğini anlamak için `intterrupted()`metodu (bu arada statik bu metod) veya `isInterrupted()` metodu ile öğreniriz.

Dikkat edilmesi gereken `interrupted()` metodu ilk çağırıldığında gerçekten bir kontrol yapar bir rahatsız edilme durumu varmı yok mu kontrol eder ve daha sonra işareti kapatır fakat ikinci çağırılma da bu işlemi yapmaz işareti kapattık bize false dönecektir ve kontrol doğru yapılmamış olacaktır.

`isInterrupted()` metodu ise çalıştıktan sonra işareti kapatmaz ve sürekli bir kontrol yapabilir.

`sleep()` ile threadi uyuttuk artık aktif değil. Ready de bekleyen threadlere forma şansı doğmuş olur böylece

Bir thread çalışıyor diyelim ki biz onu değil de aynı önceliğe sahip başka bir thread çalıştırmak istiyoruz onun için  `yield()` kullanırız 
Şimdi şey diyebilirsiniz la biz `sleep()` ile de başkasını çalıştırabiliyorduk nereden çıktı bu, niye bunu kullanıyım ki? 
Aralarında ki fark şöyle;
`sleep()` daha düşük öncelikli threadlere de forma şansı verebilmekte yield() ise ancak aynı önceliklileri çalıştırmaktadır.

`yield()` statiktir bunu kullanmak için nesneye falan ihtiyaç yoktur class üstünden çağırabiliriz.

Ee ya öncelik sıraları aynı olmasaydı bu threadlerin ? O zaman `yield()` kullanmak pek akıllıca olmayacak çünkü `yield()` aynı önceliğe sahip olanları çalıştırmaktadır.
Ee aynı öncelik sırası olmadığı için yine sırayla threadler çalışacaktır fuzili birşey olacaktı.

**`isAlive()`**
Meali: Yaşıyor mu bizim thread? 
true or false döner. 
**KAMUSPOTU**
Unutmayalım `sleep()` edilen thread ölü değildir. 

**`join()`** 
Meali: Threadin hadi bekliyorum dediği durum.
 
Thread tasarımı iki şekilde yapılıyor Biri `java.lang.Thread` sınıfını miras almak, diğeri `runnable` arayüzünü implement etmek.
Birinci yolun dezavantajı javada **single inheritance** olayı. Thread sınıfını kalıttık diyelim ee bizim başka bir sınıfı kalıtmaya ihtiyacımız olsa n'olacaktı?

Java da 2 tane thread çeşidi var biri `user threads` biri `daemon threads`farklarına gelirsek önce bağımlı thread bağımsız thread nedir bi bakalım threadi oluşturan thread sonlansada oluşan thread hayatına devam ediyorsa bunlara bağımsız thread deniliyor oluşturulan her threadin türü default olarak `bağımsızdır`mantıken bağımlı olan türümüz de ata thread e bağlı oluyor.

bizim threadlerden `user thread bağımsız daemon threads` ise `bağımlı thread oluyor.`
`setDaemon(t/f) `kullanarak bağımlı bağımsız hale getiriyoruz.

`synchronized` critical section bölgesine aynı anda sadece tek bir threadin erişmesini sağlar. Bu synchronized bizim metodu tanımlarsa metod tamamen koruma altında olur.
Bir scope olarak tanımlanırsa scop koruma altında olur.

2 thread 1 nesne ve nesnenin içinde 1.metod ve 2.metod adında metodlar olsun. Bu iki metod synchronized ile tanımlı. Birinci thread nesnenin 1.metodunu çağırdı diyelim 2.thread 2.metodu çağırmak istedi ama başaramadı çünkü 1.threadin metod çağrısı bunu engelledi bunun nedeni `synchronized` kelimesi. La oğlum ne alaka biz 2.metoda erişmedik ki onun kilidi boşta diyebilirsiniz
fakat **`HER NESNENİN TEK KİLİDİ VARDIR`** kilit 1.thread de olduğu için 2.thread 1.nin 1.metodla işinin bitmesini bekleyecek.

Her nesnenin bir bekleme havuzu(`object's monitor`) vardır. `wait()` ile threadler buraya yollanır. Buradan `notify()` veya `notifayAll()` ile havuzdan atılır.
**`Wait()`**
Threadin nesneyi havuza alma ve çıkarma işlemlerini yapabilmesi için nesne kilidine sahip olması gerekir.
`wait()` kullanarak havuza aldığımız thread sahip olduğu kilidi bırakır.
`wait()` metodu başka bir thread `notify()` veya `notifyAll()` çağrısı yapana kadar threadi bekletir.

Bunun 2 tane daha şekli var overload edilmiş halleri işte süreye kadar bekleme olayları veya `notify()` veya `notifyAll()` çağrısı durumları.

**`notify()`**
Threadi bekleme havuzundan çıkarmaya yarar bu metod rastgele bir threadi havuzdan çıkartıyor.
Bunu kullanırken kilide ihtiyacımız olur .

**`notifyAll()`** 
tüm threadleri havuzdan çıkartır.
kilit ihtiyacı var.

Statik metodlara erişim nesne üstünden yapılmaz sınıf üstünden yapılır bu durumu kontrol altına almak için `sınıf kilidi` olabilir.
Sınıfın kilidi bir tanedir. (Üzmeyelim)

> Kullandığım Kaynak:
> Java Programlama Dili ve Yazılım Tasarımı , Altuğ B. Altıntaş
