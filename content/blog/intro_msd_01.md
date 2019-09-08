---
title: Yapılandırılmış Bilgiyle Yönetime Giriş
date: 2017-11-24T12:12:52+03:00
draft: false
description: ""
tags: ["management_by_structured_data"]
categories: ["business", "management_by_structured_data"]
type: post
url:
---

Azerbaycan'ın Tapu Kadastro Genel Müdürlüğünün kurumsal otomasyonunu yaptık. 12 tane business process vardı. Alışıldık kurumsal yazılımlara ek olarak, sistemin GIS tarafı da vardı. GIS, harita üzerinde geometrik bilgilerin tutulmasını gerektiren coğrafi bilgi sistemlerinin kısaltmasıdır.

<!--more-->

<!-- toc -->

Bu projede, alışıldık yazılım projelerinden farklı bir yöntem kullandık, gereksinimlerin analizi ve dokümantasyonunda. Bu kullandığımız yöntemin, bize çok fayda sağladığını düşünüyorum. Normalde iyimser bir tahminle belki 2-3 senede bitecek bir yazılım projesini, 1 sene içinde tamamlayabildik. 
 
Alışıldık gereksinim analiz yöntemleri use caseler, user story'ler, mockup'lar, fonksiyonel ve non-fonksiyonel gereksinimler, post-it tabanlı çeşitli agile yöntemleri gibi araçları içerir. Bunlar kötü değil. Biz de bunları kullandık. Fakat bunlara ek olarak burada size anlatacağım bilgiyi yapılandırmaya dayalı bir proje yönetim yöntemini de kullandık. Bu yöntemin büyük fark yarattığı kanaatindeyim.

### Bilgiyi Yapılandırmak Nedir

Bilgiyi yapılandırmaktan kastım ne, önce onu açıklayayım. Biz insanların kendi aramızda iletişim için kullandığımız ifadeler, serbest formatta, yapılandırılmamış bilgilerdir. Use caseler, user storyler bir şablon kullanılsa da kullanılmasa da, yapısal olayan, serbest formatta yazılmış metinlerdir. Mockuplar ve UML şemaları ise yine serbest formatta çizilmiş şemalardır. Bunların hepsi değerli ve birbirimizle iletişim için kullanışlı araçlar. Fakat yapısal bilgi bunlardan farklı. Yapısal bilgi, örneğin SQLdilini kullanan ilişkisel veritabanlarında veya XML/Json formatlarına dayanan gibi hiyerarşik veritabanlarında tutulan bilgilerdir.

Yapısallıktan kasıt, farklı bilgilerin arasında bir ilişkinin tutulmasıdır. Örnek üzerinden anlatayım. Bir okulun bilgi sistemini düşünelim. Veritabanında öğrenci, öğretmen ve derslerin bilgilerini tutuyoruz. Öğrenciler için bir liste, öğretmenler için bir liste, dersler için de ayrı bir listemiz var. Bu 3 liste birbirinden ayrı listeler olarak kalsaydı, herhangi bir yapıdan bahsetmezdik. Fakat bunların arasında ilişkiler vardır ve bizim veritabanımız bu bilgileri de saklar. Örneğin, belirli bir dersi hangi öğretmen vermektedir? Belirli bir derse hangi öğrenciler kayıtlıdır? Bu iki bilginin türü, ilişkisel bilgidir. "Bir dersi veren öğretmen" bilgisi, ders ve öğretmen tabloları arasında bir ilişkiye aittir.

Yapı kavramı nereden geliyor? Neden buna yapılandırılmış bilgi diyoruz? Çünkü farklı bilgileri birleştiren bir şey inşa ediyoruz. Birleştirmek burada kilit kavram. Bilgi parçacıkları birbirinden bağımsız parçacıklar değil. Birbiriyle birleşebilen yapıtaşları. Aynı bir binayı yapıtaşı olan tuğlalardan inşa ettiğimiz gibi bir veritabanını da onun yapıtaşı olan bilgi parçalarından inşa ediyoruz.

### Yapılandırılmış bilginin önemi

Bu neden önemli? Neden yapılandırılmış bilgi muazzam faydalı bir şey? Çünkü bütün, parçaların toplamından daha fazla olan bir şeydir. Bu söz biraz felsefi ve spekülatif bir cümle gibi kulağa gelebilir. Bu türden felsefi içeriği olan ifadelere karşı özellikle şüpheci bir şekilde yaklaşmayı yeğliyorum. Çünkü bu tür cümleler insana havalı geliyor. Havalı geldiği için de, insan bunları otomatikman benimsemeye meylediyor. Bunları ortamlarda kullanarak, kendisine iyi bir imaj oluşturmak için kullanıyor. Bir sürü insan, karizmasını ve servetini böyle havalı sözleri uygun zamanda söyleyebilmesine borçlu. Fakat bu yanıltıcı bir tutum. Bu yüzden, böyle felsefi sözlere karşı özellikle sorgulayıcı yaklaşmayı tercih ediyorum.

Peki bütün gerçekten parçalarının toplamından daha fazla olabilir mi? Bu cümle ne demektir? Böyle bir söze ihtiyaç var mı? Yoksa zaten ek bir anlam katmayan bir süsleme cümlesi mi?

Çok basit bir örnek üzerinden düşünelim. Bir öğretmen listesi var. Bir de ders listesi var. Birinci durumda, bu iki liste arasında herhangi bir ilişki bilgisi yoktur. İkinci durumdaysa, ilişki bilgisi de kayıtlıdır. 

Birinci durumda, elimizde birbirinden kopuk, izole iki tane liste var. İkinci durumdaysa, iki farklı liste ve bunların arasındaki ilişki var. İkinci durumda, ekstra bir bilgi saklıyoruz. İlişkileri de ayrı bir listede tuttuğumuzu düşünelim. Bu durumda, ilk durumda elimizde 2 liste varken, ikinci durumda elimizde 3 liste var. 

Şöyle bir soru akla geliyor: Elinde 3 tane liste varsa, bu doğal olarak, 2 listeden daha fazla bilgi içerir. Bu durumun, bütünün parçaların toplamından fazla olmasıyla ne ilgisi var?

Evet, bu çok doğru bir itiraz. Bütünün parçaların toplamından fazla olması, meselesi bilginin miktarıyla ilgili bir cümle değil. Bu cümle, `2 + 1 > 3` demek gibi bir şey. Neden? Bir örnek üzerinden yine anlatayım.

Mesela, şöyle bir soru aklınıza geldi: Hiçbir ders vermeyen öğretmenler kimlerdir? Veya beşten fazla ders veren öğretmenler kimlerdir? Herhangi bir öğretmeni bulunmayan ders var mıdır? Bu türden sorular, yapılandırılmış bilgilere ihtiyaç duyan sorulardır. 

Bu tür sorularla, örneğin şu tür soruları kıyaslayalım: Yaşı 30'un altında olan öğretmenler kimlerdir? En uzun isme sahip öğretmenler kimlerdir? İçinde "Giriş" kelimesi geçen dersler nelerdir? Bu tür sorular, yapılandırılmış bilgiye ihtiyaç duymaz. Elinizde birinci durumdaki gibi birbiriyle ilişkilendirilmemiş öğretmenlerin ve derslerin listesinin olması yeterli bu sorulara yanıt vermek için.

[Neden yapılandırma felsefesini uzun uzun anlatıyorum?](/management/neden_yapilandirma_felsefesini_uzun_uzun_anlatiyorum)

İşte bütünün parçaların toplamından fazla olması, bu tür ilişkisel bilgilerin ortaya çıkardığı imkan sayesinde oluyor. İlişkisizlik durumunda, sahip olduğunuz listelerin sayısının artması, o listelerin her birinin yanıtlayabildiği sorular kadar toplam bilginizi genişletir. İlişkililik durumundaysa, listelerin sayısı arttıkça, listelerin kendi başlarına yanıtlayabildikleri sorulardan çok daha fazlasını, bilgi sistemi yanıtlamaya izin verir. İşte `2 +1 >3` ifadesinden kasıt böyle bir şey. 

Hatta bu bilgi genişlemesi o kadar hızlı ve üretkendir ki, bilgi sistemine yeni listelerin eklenmesi, kombinatorik bir hızla büyüyen bir bilgi artışına neden olur. Çünkü bir sistemdeki parçaların sayısı arttıkça, onların arasındaki olası ilişki kombinasyonlarının sayısı kombinatorik bir hızla büyür. 

