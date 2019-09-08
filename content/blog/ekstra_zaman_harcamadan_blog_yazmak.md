---
title: "Ekstra Zaman Harcamadan Blog Yazısı Yazmak"
date: 2018-01-22T13:39:55+03:00 
draft: false
description: ""
tags: ["blog", "rmarkdown"]
categories: ["software", "rmarkdown"]
type: post
url:
author: "Mert Nuhoglu"
output: html_document
blog: mertnuhoglu.com
resource_files:
- 
path: ~/projects/jekyll/mertnuhoglu.com/content/tech/ekstra_zaman_harcamadan_blog_yazmak.md
state: wip
lang: tr

---

Bir süredir `Rmarkdown` tabanlı bir araç olan `blogdown` kullanıyorum. Bu araç, neredeyse hiç ekstra zaman harcamadan, mevcut programlama işlerimi yaparken, blog yazıları da yayınlamamı sağlıyor. Tabi bir şartla: Yaptığım işin bizzat kendisini blogluyorsam eğer. 

<!--more-->

Bence bu güzel bir sınırlama. Çünkü en iyi bildiğimiz şey, bizzat işimizin gereği olarak yaptığımız şey neyse odur. Dolayısıyla, bu sınırlama sayesinde aslında en çok fayda sağlayabileceğimiz şeyler üzerine yazmaya otomatik olarak yönlendirilmiş oluyoruz.

Bu sınırlamanın, bir başka güzel faydası da şu: Yaptığın işi her zaman anlaşılır şekilde dokümente etmeye zorlanıyorsun. 

`Rmarkdown`, python ekosistemindeki `Jupyter` aracının R tarafındaki muadili. İşleyiş şekli olarak temel bir fark var yalnız ikisi arasında. `Jupyter` web tarayıcısı üzerinde çalışan bir araç. Bir web dokümanının içinde yazılım ve doküman yazmayı zorunlu tutuyor. `Rmarkdown` ise bildiğimiz eski klasik metin dosyalarını girdi olarak alıyor. Normalde markdown dosyaları `md` uzantılı dosyalardır. Rmarkdown da `Rmd` uzantılı dosyaları işliyor. Bu dosyaların içine kod parçacıkları eklemek mümkün. `Rmarkdown` bu kod parçacıklarını aynı `Jupyter` gibi çalıştırıp, sonuçlarını çıktı `md` veya `html` uzantılı dokümanın içine kendisi gömüyor. İşte bu çıktı `html` veya `md` dosyasını da `hugo`, `jekyll` gibi statik bir blog aracı webde yayınlıyor. 

`Jupyter`e göre fayda ve maliyetleri çok fazla değil. Bence her ikisini de öğrenip, ikisini de uygun yerde kullanmak zor değil. `Jupyter` blog yazmak için dizayn edilmemiş. Daha interaktif bir çalışma ortamı olarak dizayn edilmiş. `Rmarkdown` ise bir doküman çıktısı üretmeye biraz daha uygun. Her ikisi de ancak muhtemelen birbirinin en iyi olduğu alanlarda uygulanabiliyordur. 

Nasıl ki, `Jupyter` aslında sadece python değil, tüm programlama dillerini destekleyen bir altyapıya sahipse, `Rmarkdown` da aynı şekilde. İlk başta öğrenmek için [Rmarkdown resmi dokümantasyonunu](https://rmarkdown.rstudio.com/) ve [blogdown resmi dokümantasyonunu](https://bookdown.org/yihui/blogdown/) takip etmenizi öneririm. 

`blogdown`, `Rmarkdown` dokümanlarını popüler statik bloglama araçları `hugo`, `jekyll` ve diğerleriyle entegre etmeyi sağlıyor. 

