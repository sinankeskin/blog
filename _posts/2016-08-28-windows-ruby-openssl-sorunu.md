---
layout: post
title: Windows Ruby OpenSSL sorunu
---
# Nedir?

Ruby'de *net/http* kütüphanesi içerisinde uzun zamandır süregelen SSL sertifikasında TLS el sıkışması sırasında varsayılan olarak geçerlilik kontrolü yapılmaması sorunu var. Sorunun altında yatan nedenle uğraşmak yerine (bulunamayan sertifika yetkilisi, imzalı sertifika vb.) insanlar [kötü](http://stackoverflow.com/questions/1555006/how-do-i-tell-rubys-openssl-library-to-ignore-a-self-signed-certificate-error) [yamalarla](http://www.ruby-forum.com/topic/129530) [uğraşıyorlar](http://www.peterkrantz.com/2007/open-uri-cert-verification/). Bu da bizi [şöyle](http://www.rubyinside.com/how-to-cure-nethttps-risky-default-https-behavior-4010.html) bir probleme doğru sürüklüyor.

[Rails Installer](http://railsinstaller.org) içerisindeki OpenSSL kütüphanesinde herhangi bir sertifika yetkilileri dosyası tanımlı değil. O halde, gidip [curl](http://curl.haxx.se/ca/) sitesinden bu dosyayı alalım.

Peki en nihayetinde bunu Ruby için yaptığımıza göre neden indirip kurma işini de Ruby'e bırakmıyoruz?

# Kurulum

## Ruby ile (zevkli yöntem)

Bu yöntem bilgisayarınızda [Rails Installer](http://railsinstaller.org)'ın kurulu olduğunu varsayar.

Aşağıdaki Ruby betiğini *Masaüstü* klasörünüze kaydedin. [https://gist.github.com/raw/867550/win_fetch_cacerts.rb](https://gist.github.com/raw/867550/win_fetch_cacerts.rb).

Komut satırında bu betiği aşağıdaki gibi çalıştırın:

    ruby "%USERPROFILE%\Desktop\win_fetch_cacerts.rb"

Sonrasında Ruby'nin bu sertifika yetkilileri dosyasından haberdar olması için `SSL_CERT_FILE` değişkenini tanımlayın.

Bunu yapmak için komut satırında aşağıdaki komutu çalıştırın:

    set SSL_CERT_FILE=C:\RailsInstaller\cacert.pem

Bu değişikliği kalıcı hale getirmek için değişkeni [kontrol paneli](http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/environment_variables.mspx?mfr=true)'nden ortam değişkenlerine ekleyin.

## Manuel olarak (sıkıcı yöntem)

`cacert.pem` dosyasını [http://curl.haxx.se/ca/cacert.pem](http://curl.haxx.se/ca/cacert.pem) adresinden indirin. İnen dosyayı `C:\RailsInstaller\cacert.pem` yoluna kaydedin.

Ruby'nin bu sertifika yetkilileri dosyasından haberdar olması için `SSL_CERT_FILE` değişkenini tanımlayın.

Bunu yapmak için komut satırında aşağıdaki komutu çalıştırın:

    set SSL_CERT_FILE=C:\RailsInstaller\cacert.pem

Bu değişikliği kalıcı hale getirmek için değişkeni [kontrol paneli](http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/environment_variables.mspx?mfr=true)'nden ortam değişkenlerine ekleyin.

**Yazar ve Kaynak**

Original gist: [Download a cacert.pem for RailsInstaller by Fletcher Nichol][gist]

[gist]: https://gist.github.com/fnichol/867550
