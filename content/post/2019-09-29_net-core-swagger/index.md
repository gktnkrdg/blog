---
title: "NET Core Swagger Kullanımı"
author: "Gökten Karadağ"
date: 2019-09-29T14:07:05.622Z
lastmod: 2019-10-05T18:55:21+03:00
slug : "net core swagger kullanimi"
description: "OpenAPI nedir, Swagger Swashbuckle nedir ve Swagger’ın .NET Core 3 için kullanımı"
tags : [
    ".net",
    ".net core",
    "swagger",
    "open api",
]
toc: true
draft: true
subtitle: "Günümüzde birçok API tasarlıyoruz ya da kullanıyoruz. Bu API’ların anlaşılabilir olması , iyi yazılmış bir dökümantasyona sahip olması ve…"





---

# Giriş
Günümüzde  birçok API tasarlıyoruz ya da kullanıyoruz. Bu API’ların anlaşılabilir olması , iyi yazılmış bir dokümantasyona sahip olması ve bu dokümantasyonun güncel kalabilmesi oldukça önemli. Tam bu noktada OpenAPI şartnamesi hem insanların hem de bilgisayarların; kaynak koda, dokümantasyona bağlı kalmadan RESTful API’ları anlayıp işlemesi için standart bir tanımlama sunuyor. Bu tanımlama ile API’a ait bütün endpointleri, tüm metotları, metotların input, outputlarını ve authentication bilgilerini görebilmeye imkân veriyor.





{{< figure class="center" src="/image/net-core-swagger/1.png#center" title="OpenAPI 3.0 ile Open 2.0 arasındaki karşılaştırma" >}}




Swagger ise bu [OpenAPI](https://swagger.io/resources/open-api/) şartnamesi etrafında oluşturulmuş REST API’leri oluşturmanıza, tasarlamanıza ve kullanmanıza yarayan bir dizi açık kaynaklı araç topluluğu.

Swagger’ın sunduğu bazı araçlar ise şöyle:

*   [**Swagger Editor**](http://editor.swagger.io/)**:** OpenAPI belgesini anlık olarak düzenleyip ön izlemenizi sağlar.
*   [**Swagger UI**](https://swagger.io/swagger-ui/): OpenAPI belgesini interaktif bir HTML CSS dökümantasyona çevirir.Bu ara yüz üzerinden API’ı test etmenize olanak sağlar.
*   [**Swagger Codegen**](https://github.com/swagger-api/swagger-codegen): Hem client hem de server tarafında kod üretmenizi sağlar.

# Kurulum
Birçok programlama dili için Swagger kütüphanesi bulunuyor. Swashbuckle ise .NET üzerindeki en popüler Swagger kütüphanelerinden birisi.Diğer diller için geliştirilen kütüphaneleri ve alternatifleri [buradan](https://swagger.io/tools/open-source/open-source-integrations/) inceleyebilirsiniz.Swashbuckle’ın **.NET Core 3** için kurulumu aşağıdaki gibidir.

``Install-Package Swashbuckle.AspNetCore -Version 5.0.0-rc3``

> .NET Core versiyonları arasında kurulumda farklılıklar gözlenebilir.Kurulum için ayrıca [buradan](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) yararlanabilirsiniz.

Swashbuckle.AspnetCore 5.0 versiyonuyla beraber OpenAPI 3'ü desteklenmeye başlamıştır. Versiyonlar arasındaki farkları detaylı olarak [buradan](https://blog.readme.io/an-example-filled-guide-to-swagger-3-2/) inceleyebilirsiniz.

Kurulum tamamlandıktan sonra

**Startup.cs** altına aşağıdaki kod blokları eklenir.
{{< gist gktnkrdg 345d5d0ee01d0c6d66047c360fb47f88 >}}



Ardından Swagger middleware’ini eklemek için aşağıdaki kod bloğu eklenir.
{{< gist gktnkrdg 6d80891526275adf02069870e13f5dd4 >}}



Projemizi çalıştırdıktan sonra adres satırında **/swagger** altında swagger ara yüzünün geldiğini görebiliriz.




{{< figure class="center" src="/image/net-core-swagger/2.png#center" title="Swagger Arayüzü " >}}

# Custom Header
Eğer API projenize bir header eklemek ve bunu Swagger’a dahil etmek isterseniz yapmanız gereken bir operation filter tanımlamaktır. Aşağıda **.NET Core 3** için örnek kullanımını görebilirsiniz.
{{< gist gktnkrdg 7b672da3bae08abe5621390cc026ff59 >}}



Sonrasında AddSwaggerGen metotunda güncelleme yapılması gerekir.

{{< gist gktnkrdg 9f037c5c24de57b9f168d1f64d91aa40 >}}

Değişiklikleri tamamladıktan sonra artık Swagger ui üzerinden header alanımızın input olarak girilebildiğini görebiliriz.



![image](/image/net-core-swagger/3.png#center)

Swagger ile anlatacaklarım şimdilik bu kadar. Bir sonraki makalede görüşmek üzere.

**Github** : [https://github.com/gktnkrdg/SwaggerCore](https://github.com/gktnkrdg/SwaggerCore)

# Referanslar

[OpenAPI Specification](http://spec.openapis.org/oas/v3.0.2)

[OAI/OpenAPI-Specification](https://github.com/OAI/OpenAPI-specification/)

[Swashbuckle ve ASP.NET Core kullanmaya başlayın](https://docs.microsoft.com/tr-tr/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-3.0&amp;tabs=visual-studio)

[domaindrivendev/Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)
