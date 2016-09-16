---
layout: post
title: Windows komut satırınızı renklendirin
comments: true
---
Windows harici işletim sistemlerinde [ANSI renk kaçış kodları](https://en.wikipedia.org/wiki/ANSI_escape_code) tümleşik olarak bulunuyor.
Özellikle Ruby on Rails ile çalışırken öncelikle göze çok hoş görünüyor sonrasında da farkındalığı arttırarak geliştiriciye fayda sağlıyor.

Örneğin Linux Mint uçbirimde yeni bir Ruby on Rails uygulaması oluştururken aşağıdaki gibi bir sonuç elde ediyoruz. Çarpıcı.

![Linux Mint](/assets/images/linux_cmd.png)

Oysa varsayılan olarak aynı şeyi Windows komut satırında yaptığımızda ise elde ettiğimiz sonuç aşağıdaki gibi. Hüsran.

![Windows Renksiz](/assets/images/windows_renksiz_cmd.png)

İşte tam bu aşamada yardımımıza [ANSICON](https://github.com/adoxa/ansicon) koşuyor. Açık kaynak kodlu bir uygulama olan ANSICON tabii sadece Rails değil tüm ANSI renk kaçış kodu kullanan konsol uygulamalarında devreye giriyor.

Kurulum ve kullanım için öncelikle yapmanız gereken yayınlanan [versiyonlar](https://github.com/adoxa/ansicon/releases) linkinden son versiyonu indirmek olmalı. Sonrasında işletim sisteminizin mimarisine göre 32 bit (x86) ya da 64 bit (x64) klasöründeki dosyaları komut satırının ulaşabileceği bir yere çıkartın.

Eğer dosyaları işletim sisteminin doğrudan ulaşabileceği bir klasöre çıkarttıysanız komut satırında

    ansicon -i

yazarak ANSICON'un kendini AutoRun'a kopyalamasını sağlayabilirsiniz.

Eğer dosyaları işletim sisteminin doğrudan ulaşamayacağı bir klasöre çıkarttıysanız komut satırında klasörün tam yolunu yazarak

    C:\Users\Test\Desktop\ansicon -i

ANSICON'un kendini AutoRun'a kopyalamasını sağlayabilirsiniz.

Komut satırını bir sonraki açışınızda ANSICON kendini yükleyecektir ve artık Windows üzerinde Ruby on Rails uygulamaları oluştururken konsol Linux'deki gibi renklenecektir.

![Windows Renkli](/assets/images/windows_renkli_cmd.png)

Artık ANSI renk kaçış kodu kullanan tüm konsol uygulamaları Windows'ta da doğru görünecektir.

Son olarak Powershell'de çalıştıracağınız konsol uygulamalarınızın da renkleneceğini söyleyerek bitirelim.

İyi günler.
