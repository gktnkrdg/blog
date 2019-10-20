---
title: "S.O.L.I.D Prensipleri -Nedir  #1"
author: "Gökten Karadağ"
date: 2019-10-19-T14:07:05.622Z
lastmod: 2019-10-19T18:55:21+03:00
slug : "solid prensipleri"
tags : [
    "S.O.L.I.D",
    "solid",
    "yazılım prensipleri",    
    "nedir"
]
description: "Solid Prensipleri nedir"
draft: false 
---
# Nedir
S.O.L.I.D Prensipleri, Robert C. Martin tarafından hazırlanan Nesneye Tabanlı Programlama Prensiplerinin ilk 5 maddesinin kısaca [adlandırılmasıdır](http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod).

Bu maddeler

**S** – (SRP) Single responsibility principle 

**O** – (OCP) Open closed principle

**L** – (LSP) Liskov substitution principle

**I** –  (ISP) Interface segregation principle

**D** – (DIP) Dependency Inversion principle

S.O.L.I.D prensipleri , geliştirilen yazılımların daha anlaşılır olmasını ve bakımının ve daha kolay olmasını hedefler.

Peki nedir bu prensipler , tek tek inceleyelim.
![](/image/solid-prensipleri/solid-1.png#center)
# Single Responsibility Principle

> Bir sınıfın veya metodun tek bir sorumluluğu olmalıdır.

Örneğin SendEmail isimli bir metotumuz olsun.
{{< highlight cs >}}
 public class EmailService
    {
    	public void SendEmail(string email, string message)
      {
      	  if(!email.Contains("@") || !email.Contains("."))
          {
          	throw new Exception("Email is not valid!!");
          }
          SmtpClient client = new SmtpClient();
          client.Send(new MailMessage("test@email.com", email) { Subject = "Test!" });
      }
    }
{{< /highlight >}}

SendEmail metotu hem emaili doğruluyor hem de email gönderim işlemini yapıyor. Metotun birden fazla sorumluluğun olması Single Responsibility Principle ile çelişiyor.Bu yüzden bu metotu prensipe uygun olacak şekilde düzenleyelim ve SendEmail metotunu sadece mail göndermeden sorumlu hale getirelim.
{{< highlight cs >}}
    public class EmailService
    {
        public void ValidateEmail(string email)
        {
            if (!email.Contains("@") || !email.Contains("."))
            {
                throw new Exception("Email is not valid!!");
            }
        }
        public void SendEmail(string email, string message)
        {
            ValidateEmail(email);
            SmtpClient client = new SmtpClient();
            client.Send(new MailMessage("test@email.com", email) { Subject = "Test mail subject!" });
        }
    }
{{< /highlight >}}
# Open/Closed Principle

> Classlar,metotlar değişikliğe kapalı ancak gelişime açık olmalıdır.

Geometrik şekillerin alanlarını hesaplayan bir yapımız olsun.
{{< highlight cs >}}
    public class Rectangle
    {
        public double Width { get; set; }
        public double Height { get; set; }
    }
    public class Circle
    {
        public double Radius { get; set; }
    }
    public class CombinedAreaCalculator
    {
       public double Area(object[] shapes)
        {
            double area = 0;
            foreach (var shape in shapes)
            {
                if (shape is Rectangle)
                {
                    Rectangle rectangle = (Rectangle)shape;
                    area += rectangle.Width * rectangle.Height;
                }
                if (shape is Circle)
                {
                    Circle circle = (Circle)shape;
                    area += (circle.Radius * circle.Radius) * Math.PI;
                }
            }
            return area;
        }
    }
{{< /highlight >}}

Yeni bir şekil eklemek ve bunun da alanını hesaplamak istediğimizde  CombinedAreaCalculator metodumuzu değiştirmemiz gerekecek . Fakat classlarda metotlarda yapacağımız geliştirmeler  değişikliğe sebep olmamlı. Peki bunu en basit haliyle nasıl düzenleyebiliriz. Öncelikle Shape isimli bir abstract class tanımlayalım.
{{< highlight cs >}}
    public abstract class Shape
    {
        public abstract double Area();
    }
{{< /highlight >}}
Ardından Rectangle ve Circle classlarımızı bu Shape class'ından türetelim ve Area metotlarını bu classlara göre düzenleyelim.
{{< highlight cs >}}
    public class Rectangle : Shape
    {
        public double Width { get; set; }
        public double Height { get; set; }
        public override double Area()
        {
            return Width * Height;
        }
    }
    
    public class Circle : Shape
    {
        public double Radius { get; set; }
        public override double Area()
        {
            return Radius * Radius * Math.PI;
        }
    }
{{< /highlight >}}
Son olarak CombinedAreaCalculator class'ını güncelleyelim. Artık yeni bir şekil geldiğinde sadece yeni bir class oluşturacak CombinedAreaCalculator metotunu değiştirmemize gerek kalmadan geliştirmemizi tamamlayabiliriz.

{{< highlight cs >}}
    public class CombinedAreaCalculator
    {
        public double Area(Shape[] shapes)
        {
            double area = 0;
            foreach (var shape in shapes)
            {
                area += shape.Area();
            }
            return area;
        }
    }
{{< /highlight >}}


# Liskov's Substitution Principle

> Bir base class'tan türetilen class'lar üst class'ların yerine de kullanabililir olmalıdır.

Bird classımız var ve içine Fly metotu ekledik. Ardından Eagle(kartal) ve Ostrich(devekuşu) bird'den türettik. Fakat base class türetilen classlar üst classların yerine de kullanılabilmeli. Ostrich bir üst clasının özelliği olayı fly metotunu sağlayamıyor.Bu yüzden kodumuzda değişiklik yapmamız gerekiyor.
{{< highlight cs >}}
    class Bird{
        void Fly() {
        
        }
    }
    class Eagle : Bird {
    
    }
    class Ostrich : Bird {
    
    }
{{< /highlight >}}
FlyingBirds isimli yeni bir class oluşturalım ve Fly metotunu bu class'a ekleyelim. Artık herbir alt class'ı bir üst class'ının yerine kullanabiliriz.
{{< highlight cs >}}
    class Bird{
    
    }
    class FlyingBirds : Bird{
        void Fly(){
        
        }
    } 
    class Eagle : FlyingBirds {
    
    }
    class Ostrich : Bird{
    
    }
{{< /highlight >}}

# Interface Segregation Principle

> Classlar ihtiyaç duymadıkları bir interface'i kullanmaya zorlanmamalı veya ihtiyaç duyduğu interface'e ait tek bir metod için interface'in bütün metotlarını implemente etmek zorunda kalmamalıdır.

ToyHouse isimli bir class'ımız olsun ve bunu IToy interface'inden türetelim.
{{< highlight cs >}}
    interface IToy {
    	void SetColor(string color);
    	void Fly();
    }
    class ToyHouse :IToy {
    	String color;
    	public void SetColor(string color) {
    		this.color = color;
    	}
    	public void Fly(){
    	  throw new NotImplementedException();
    	}
    }
{{< /highlight >}}
ToyHouse class'ı IToy interface'inden türetildiği için Fly metotuna kullanmaya zorlanıyor. Fakat ToyHouse'ın uçabilme gibi bir özelliği yok. 

Burada çözüm IToy interface'inden Fly metotunu ayırmak, IFlyable isimli bir interface yaratmak olabilir.  
{{< highlight cs>}}
    interface IToy {
    	void SetPrice(double price);
    }
    interface IFlyable {
       void Fly();
    }
{{< /highlight >}}
Ardından kodumuzu düzenlersek , daha doğru bir kod elde etmiş oluruz.
{{< highlight cs >}}
    class ToyHouse :IToy {
    	double price;
    	public void SetPrice(double price) {
    		this.price = price;
    	}
    }
    
    class ToyPlane : IToy, IFlyable
    {
        double price;
        public void SetPrice(double price)
        {
            this.price = price;
        }
        public void Fly()
        {
            //
        }
    }
{{< /highlight >}}


# D**ependency Inversion Principle**

> Üst modüller alt seviyedeki modüllere doğruda bağımlı olmamalıdır.soyutlamalar ile bağlı olmalıdır.Alt modüllerde yapılan bir değişiklik üst modülü etkilememelidir..

Post kaydı oluşturduğumuz bir yapımız olsun .

{{< highlight cs >}}
    class Post
    {
        private FileLogger errorLogger = new FileLogger();

        void CreatePost( string postMessage)
        {
            try
            {
                // create post
            }
            catch (Exception ex)
            {
                errorLogger.Log(ex.ToString());
            }
        }
    }
    class FileLogger
    {
        public bool Log(string message)
        {
            return true;
        }
    }
{{< /highlight >}}
Fakat üst seviye modülümüz olan CreatePost metotu FileLogger metoduna bağlı.FileLogger'da değişiklik yapmamız gerekirse Post class'ını da değiştirmemiz gerekecek. Burada sorunu  dependency injection kullanarak çözmeye çalışalım
{{< highlight cs >}}
  
    class Post
    {
        private ILogger _logger;

        public Post(ILogger logger)
        {
            _logger = logger;
        }

        void CreatePost(string postMessage)
        {
            try
            {
                //create post
            }
            catch (Exception ex)
            {
                _logger.Log(ex.ToString());
            }
        }
    }
    interface ILogger
    {
        bool Log(string message);
    }

    class FileLogger : ILogger
    {
        public bool Log(string message)
        {
            return true;
        }
    }
{{< /highlight >}}

Bu şekilde Post class'ı FileLogger'a doğrudan bağımlılığını kaldırmış olduk.

# Sonuç

Yazılım geliştirme prensiplerinden S.O.L.I.D'in ne olduğunu incelemeye çalıştık.

Tüm bu örneklerde gördüğümüz prensiplere uygun kod yazmamız bize şu faydaları sağlar:

- Kod okunabilirliği arttır .

- Kod üzerinde kolayca değişiklik yapabilmeyi sağlar.

- Daha kolay unit test yazabilmesine imkan verir.

Kod örneklerini [Github](https://github.com/gktnkrdg/S.O.L.I.D) üzerinden inceleyebilirsiniz.

Bir sonraki yazıda görüşmek üzere.

### Referanslar

[http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod](http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)

[https://www.monterail.com/hubfs/PDF content/SOLID_cheatsheet.pdf](https://www.monterail.com/hubfs/PDF%20content/SOLID_cheatsheet.pdf)

[https://web.archive.org/web/20150906155800/http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf](https://web.archive.org/web/20150906155800/http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf)

[https://exceptionnotfound.net/simply-solid-the-single-responsibility-principle/](https://exceptionnotfound.net/simply-solid-the-single-responsibility-principle/)

[https://programmingwithmosh.com/javascript/solid-5-principles-of-object-oriented-design-every-developer-must-learn](https://programmingwithmosh.com/javascript/solid-5-principles-of-object-oriented-design-every-developer-must-learn/)

[https://itnext.io/solid-principles-explanation-and-examples-715b975dcad4](https://itnext.io/solid-principles-explanation-and-examples-715b975dcad4)
