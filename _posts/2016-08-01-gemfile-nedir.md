---
layout: post
title:  "Gemfile nedir?"
date:   2016-08-01 00:00:00 +0200
categories: jekyll update
---
Ruby geliştiricileri sıklıkla ``Gemfile`` kullanır ve bir çoğumuz temel özelliklerini biliriz. Bu makalede ``Gemfile`` için inebildiğimiz kadar derine inmeye çalışacağız.

**Gemfile nedir?**

Bir ``Gemfile`` Ruby programlarımızdaki gem bağımlılıklarını tanımlamak için kullandığımız bir dosyadır. Bir gem ise birçok Ruby dosyasından oluşan ve daha sonra kullanabileceğimiz bir kod koleksiyonudur.

``Gemfile`` dosyanız her zaman projenizin kök dizininde bulunmalıdır. Burası dosyayı ararken varsayılan olarak ``Bundler``'ın ve diğer herhangi bir paket yöneticisinin ilk bakacağı standart konumdur.

``Gemfile``'ın bir Ruby kodu olarak değerlendirileceğini bilmek faydalı olur. ``Gemfile`` içeriği Bundler tarafından değerlendirildikten sonra gem bağımlılıklarımızdaki belli metotlara ulaşabilmemize olanak sağlayacaktır.

**``Gemfile`` ayarlamak**

İlk yapmamız gereken şey ``Gemfile``’a gem’leri arayacağı yeri söylemek olacaktır. Bu da (``source``) kaynak belirtmektir. Bunun için ``source`` metodunu kullanırız.

{% highlight ruby %}
source "https://rubygems.org"
{% endhighlight %}

Projeler için birden fazla kaynak belirtmek önerilmez. %99 ihtimalle projenizin ``Gemfile``’ında kaynak olarak ``https://rubygems.org`` belirtilmesi yeterli olacaktır. Kaynak için tek şart geçerli bir Rubygems deposu olmasıdır.

**Kaynak öncelik sırası**

Şu anda kaynaklar için öncelik sırasından bahsetmenin tam sırası.

Her ne kadar ``Gemfile``'ın ilk satırında kaynak belirtebilsek bile yüklemek istediğimiz her gem için ayrı ayrı kaynak da belirtebiliriz. Ayrıca yerel bilgisayarımızda bulunan bir gem için dizin yolunu ya da GitHub gibi gem yayınlanan bir alanın adresini belirtebiliriz (Daha sonra bu konudan bahsedeceğiz).

Bundler gem'i aramaya başlamadan önce ilk olarak gem için açıkça bir kaynak belirtilip belirtilmediğine bakar.

Eğer kaynak, yol ya da git adresi belirtildiyse gem ilk olarak burada aranacaktır.

Eğer açıkça herhangi bir kaynak belirtilmediyse Bundler ilk satırda belirtilen kaynakta aramaya başlayacaktır.

Eğer gem birden fazla kaynakta bulunursa (ki bu nadiren gerçekleşir çünkü yalnızca bir adet kaynak belirtmelisiniz) hangi gem'in kullanılacağının bilinemediği konusunda bir uyarı mesajı ile karşılaşırsınız.

Kaynakları bir blok içerisinde de belirtebilirsiniz.

{% highlight ruby %}
source "https://my_awesome_source.com" do
  gem "my_gem"
  gem "my_other_gem"
end
{% endhighlight %}

**Kullanıcı bilgileri ile kaynaklar**

Bazı kaynaklar kullanıcı bilgileri ile kullanılıyor olabilir.

Bundler'ın her kaynak için kullanıcı adı ve parola belirtmenizi sağlayan bir konfigürasyon seçeneği vardır.

``bundle config my_gem_source.com my_username:my_password``

Bu kaynağı Bundler ile gem yüklemek için kullanmak isteyen herkes versiyon kontrolüne girmeden kullanıcı bilgilerini girmek zorundadır (ki bu kullanımın en iyi yanı da bu).

Kullanıcı bilgilerini tabii ki ``Gemfile`` içerisine de ekleyebilirsiniz, ancak bu şekilde bir kullanımın versiyon kontrolüne gönderilebileceğini unutmayın.

{% highlight ruby %}
source "https://username:password@my_gem_source.com"
{% endhighlight %}

Yukarıdaki örnekteki gibi, kaynak için belirtilen bilgiler Bundler konfigürasyonunda belirtilen bilgileri ezecektir.

**Ruby bilgilerini ayarlamak**

Eğer oluşturduğunuz uygulama belli Ruby versiyonuna ya da motoruna ihtiyaç duyuyorsa bunu ``Gemfile`` içerisinde belirtebilirsiniz.

{% highlight ruby %}
ruby "1.9.3", :patchlevel => "247", :engine => "jruby", :engine_version => "1.6.7"
{% endhighlight %}

Bu ayarları yapmak Ruby versiyonu hakkında belli başlı bilgilere gerek duyar (örneğimizde 1.9.3).

- :patchlevel kullanılacak Ruby yama numarasını belirtir.
- :engine kullanılacak Ruby motorunu belirtir.
- :engine_version kullanılacak motor versiyonunu belirtir. Eğer bu ayar girilirse :engine ayarının da girilmesi gerekir.

**Gem'lerin ayarlanması**

Şimdi ``Gemfile``'ın asıl görevi olan gem'lerin ayarlanmasına geldik.

En basit yazım;

{% highlight ruby %}
gem "my_gem"
{% endhighlight %} şeklindedir.

Burada my_gem gem'in ismidir. Kullanabileceğiniz bir çok seçenek vardır ancak sadece isim seçeneği zorunludur.

**Gem'in versiyonunun ayarlanması**

En sıklıkla karşılaşacağınız istek gem'in versiyonunun ayarlanması olacaktır.

Eğer herhangi bir versiyon ayarlamak istemiyorsanız temel olarak;

{% highlight ruby %}
gem "my_gem", ">= 0.0"
{% endhighlight %} şeklinde bir yazım kullanabilirsiniz.

Versiyon ayarlarken kullanabileceğiniz **7** adet operatör vardır.

- Eşit ``=`` "=1.0"
- Eşit Değil ``!=`` "!=1.0"
- Büyük ``>`` ">1.0"
- Küçük ``<`` "<1.0"
- Büyük ya da Eşit ``>=`` ">=1.0"
- Küçük ya da Eşit ``<=`` "<=1.0"
- Karamsar Büyük ya da Eşit ``~>`` "~>1.0"

**Karamsar büyük ya da eşit**

~> operatörü gem'in gelecek versiyonlarında da çalışacağını güvenli bir şekilde kontrol altına almanıza izin verir.

Eğer uygulamanıza eklemek istediğiniz gem'in tüm versiyonunun sorunsuzca çalışacağından eminseniz;

{% highlight ruby %}
gem "my_gem", "~> 2.0"
{% endhighlight %} şeklinde bir yazım kullanabilirsiniz.

Bu size herhangi bir 2.x versiyonunu yüklemenize izin verecek ancak herhangi bir 3.x versiyonunu yüklemenizi engelleyecektir.

Belki de bu kadar geniş bir versiyon aralığı vermek istemeyebilirsiniz, bu durumda daha kesin bir aralık belirtebilirsiniz.

{% highlight ruby %}
gem "my_gem", "~> 2.5.0"
{% endhighlight %}

Bu size 2.5.0 ile 2.6.0 aralığında herhangi bir versiyon yüklemenize izin verir.

Aşağıdaki tablo daha iyi anlamanıza yardımcı olabilir.

{% highlight ruby %}
gem "my_gem", "~> 1.0" –> gem "my_gem", ">= 1.0", "< 2.0"
gem "my_gem", "~> 1.5.0" –> gem "my_gem", ">= 1.5.0", "< 1.6.0"
gem "my_gem", "~> 1.5.5" –> gem "my_gem", ">= 1.5.5", "< 1.6.0"
{% endhighlight %}

**Gem'i gerekli işaretlemek**

Eğer Rails kullanıyorsanız bazı sihirler sizden saklanıyor olabilir ancak örneğin ``config/application.rb`` dosyanızda şöyle bir satır görebilirsiniz.

{% highlight ruby %}
Bundler.require(:default, Rails.env)
{% endhighlight %}

Bu satır herhangi bir gruba atanmamış ve o anki Rails ortamı ile aynı gruba (örneğin test, development) atanmış gem'lerin uygulamanıza dâhil edilmesine olanak sağlar.

``Gemfile``'a bir gem eklediğinizde bu gem ``Bundler.require`` çağırıldığında varsayılan olarak uygulamanıza dâhil edilecektir. Bunu ``require`` ayarını ``false`` yaparak engelleyebiliriz.

{% highlight ruby %}
gem "my_gem", require: false
{% endhighlight %}

Hangi klasörlerin de dâhil edilebileceğini ayrıca belirtebilirsiniz.

{% highlight ruby %}
gem "my_gem", require: ["my_gem/specific_module/my_class", "my_gem"]
{% endhighlight %}

İçerisinde çok fazla işlev bulunan bir gem'den dâhil edilecek sınıfları her seferinde manuel olarak eklemeniz gerektiğinde çok faydalı olacaktır.

**Gem'leri gruplamak**

Daha önce bahsettiğimiz gibi bir gem bir ya da birden fazla gruba dâhil olabilir. Eğer herhangi bir gruba dâhil değilse otomatik olarak ``:default`` grubuna dâhil edilir.

Gem gruplamak için iki yöntem bulunmaktadır. İlki ``:group`` özelliğini kullanmaktır.

{% highlight ruby %}
gem "my_gem", group: :development
{% endhighlight %}

Bu gem sadece development ortamında çalışılırken dâhil edilecek demektir.

Ayrıca bundle install ile gem'leri yüklerken belli grubu yüklemeyi istemediğinizde bunu belirtebilmenizi de sağlar. Yükleme süresini gözle görülür derecede azaltacaktır.

``bundle install --without development test``

Yukarıdaki satır development ya da test ortamları dışındaki tüm gem'leri yükleyecektir.

Gem'leri gruplamak için ikinci yöntem ise bir blok içerisine almaktır.

{% highlight ruby %}
group :development do
  gem "my_gem"
  gem "my_other_gem"
end
{% endhighlight %}

Bu yöntem hem göze daha hoş gelecek hem de grupları birleştirmenize olanak sağlayacaktır.

{% highlight ruby %}
group :development, :test do
  gem "my_gem"
  gem "my_other_gem"
end
{% endhighlight %}

Eğer bir grubu opsiyonel olarak işaretlemek istiyorsanız bloktan önce ``optional: true`` parametresini gönderebilirsiniz.

{% highlight ruby %}
group :development, optional: true do
  gem "my_gem"
  gem "my_other_gem"
end
{% endhighlight %}

Eğer grup yukarıdaki gibi opsiyonel olarak işaretlenmişse kullanıcı bu gem'leri sadece ``bundle install --with development`` komutları ile yükleyebilir olacaktır.

**Gem için platform ayarlamak**

Eğer bir gem sadece belli platform ya da platformlarda kullanılabilir ise bunu ``Gemfile`` içerisinde belirtebilirsiniz.

Platform belirtmek grup belirtmek ile aynı şekilde yapılır. Tek fark yükleme ``--without`` parametresini belirtmek zorunda kalmadan otomatik olarak yapılır.

{% highlight ruby %}
gem "my_gem", platform: :jruby
gem "my_other_gem", platform: [:ruby, :mri_18]
{% endhighlight %}

Gem için belirtebileceğiniz platformların listesi şöyledir.

- ruby – C Ruby (MRI) ya da Rubinius, Windows hariç
- ruby_18 ile ruby_22 arası – ruby & (versiyon 1.8 ile versiyon 2.2 arası)
- mri – Ruby ile aynı, Rubinius hariç
- mri_18 ile mri_22 arası – mri & (versiyon 1.8 ile versiyon 2.2 arası)
- rbx – Ruby ile aynı, sadece Rubinius (MRI değil)
- jruby – JRuby
- mswin – Windows
- mingw – Windows 32 bit mingw32 platformu (RubyInstaller)
- mingw_18 ile mingw_22 arası – mingw & (versiyon 1.8 ile versiyon 2.2 arası)
- x64_mingw – Windows 64 bit mingw32 platformu
- x64_mingw_20 ile x64_mingw_22 arası – x64_mingw & (versiyon 2.0 ile versiyon 2.2 arası)

Eğer geliştirme takımı farklı platformlardaysa bu ayarlar çok faydalı olacaktır. Eğer bir geliştirici Windows üzerinde geliştirme yapıyorsa destek verilen gem'leri değiştirmek isteyebilirsiniz.

Normalde platform belirlerken ben blok yazım stilini kullanıyorum.

{% highlight ruby %}
platforms :jruby do
  gem "my_gem"
  gem "my_other_gem"
end
{% endhighlight %}

**Gem için kaynak belirtmek**

Daha önce bahsettiğimiz gibi gem için kaynak belirtmek için belli bir ayar vardır.

Örnek olarak şu şekilde yazılabilir.

{% highlight ruby %}
gem "my_gem", source: "https://my_awesome_gemsite.com"
{% endhighlight %}

Eğer gem kaynakta bulunamazsa varsayılan kaynakta aranmayacak ve yüklenmeyecektir.

**Git'ten gem yüklemek**

Yükleme kaynağı olarak bir git deposunu belirtebilirsiniz (örneğin GitHub). Bu tıpkı ``:source`` parametresini kullanmak gibidir.

{% highlight ruby %}
gem "my_gem", git: "ssh@github.com/sinankeskin/my_gem"
{% endhighlight %}

Her ne kadar git deponuzu HTTP(S), SSH ve GIT protokolleri üzerinden kullanabilir olmanıza rağmen sadece HTTPS ve SSH üzerinden kullanmanız ortadaki adam (man-in-the-middle) saldırısı ile karşı karşıya kalmanızı engelleyecektir.

Eğer gem'inizi bir git deposunda saklıyorsanız içerisinde en az bir ``.gemspec`` uzantılı dosya ve bu dosyada geçerli bir gem spesifikasyonu olmak zorundadır.

Eğer bu dosyayı sağlamazsanız Bundler sizin için oluşturmaya çalışacaktır, ancak buna güvenmemelisiniz. Eğer bir git deposundan ``.gemspec`` olmaksızın bir gem yüklemeye çalışırsanız gem için bir versiyon belirtmek zorunda kalacaksınız.

Gem'iniz için isterseniz bir dal (branch), etiket (tag) ya da referans (ref) belirtebilirsiniz. Varsayılan dal ``master``'dır.

``submodules: true`` parametresini göndererek Bundler'ın alt modülleri de yüklemesini sağlayabilirsiniz.

{% highlight ruby %}
gem "my_gem", git: "ssh@githib.com/sinankeskin/my_gem", branch: test_branch, submodules: true
{% endhighlight %}

Eğer aynı git deposundan birden fazla gem yüklemek istiyorsanız bir blok kullanabilirsiniz.

{% highlight ruby %}
git "git@github.com:sinankeskin/my_gems.git" do
  gem "my_gem"
  gem "my_other_gem"
end
{% endhighlight %}

**Kaynak olarak git belirtmek**

Gem'leriniz için ortak bir kaynak oluşturan URL belirtebilirsiniz. ``#git_source`` kullanarak bunu yapabilir ve ismi göndererek tam depo adresini bloktan geri döndürebilirsiniz.

{% highlight ruby %}
git_source(:custom_git){ |repo| "https://my_secret_git_repos.com/#{repo}.git" }
gem "my_gem", custom_git: "sinankeskin/test_repo"
{% endhighlight %}

**Bitbucket ve Github yardımcı metodları**

Hem Bitbucket hem de Github popüler git depoları olduğu için yardımcı metotları bulunmaktadır.

Her iki durumda da Bundler depoların halka açık (public) olduğunu farz eder.

{% highlight ruby %}
gem "my_gem", github: "sinankeskin/my_gem"
gem "my_gem", bitbucket: "sinankeskin/my_gem"
{% endhighlight %}

``:branch`` parametresi ile belli bir dalı belirtebilirsiniz.

Eğer hem kullanıcı adı hem de depo adı aynı ise (örneğin Rails projesinde böyle) bir tanesini yazmayabilirsiniz.

{% highlight ruby %}
gem "rails", github: "rails"
gem "rails", bitbucket: "rails"
{% endhighlight %}

**Dikkat** –  Bundler 2 yayınlanana kadar ``:github`` parametresini kullanmamalısınız. Şu anda varsayılan olarak ``git://`` protokolünü kullandığı için daha önce de söylediğimiz gibi ortadaki adam (man-in-the-middle) saldırısına karşı savunmasız durumdadır.

Diğer yardımcı ise ``:gist``'dir. Bu eğer projeniz GitHub'da gist olarak tutuluyorsa faydalı olacaktır. Sadece gist ID girerek kullanabilirsiniz. ``:github`` ve ``:bitbucket`` parametrelerindeki gibi ``:branch`` parametresini metoda gönderebilirsiniz.

{% highlight ruby %}
gem "my_gem", :gist => "5935162112", branch: "my_custom_branch"
{% endhighlight %}

**Yerel gem'i yol parametresi ile dâhil etmek**

``:path`` parametesini kullanarak yerel bilgisayarınızdaki bir gem'i uygulamanıza dâhil edebilirsiniz.

{% highlight ruby %}
gem "my_gem", :path => "../my_path/my_gem"
{% endhighlight %}

Yukarıdaki gibi bağıl bir yol belirtirseniz Bundler ``Gemfile``'ın bulunduğu dizine göre gerçek yolu belirleyecektir.

Eğer birden fazla gem'in  bulunduğu bir klasörünüz varsa bunu blok ile belirtebilirsiniz.

{% highlight ruby %}
path "../my_path/gems" do
  gem "my_gem"
  gem "my_other_gem"
end
{% endhighlight %}

Bilinmesi gereken diğer bir not ``:path`` parametresi ile gem yüklendiğinde C uzantılarının derlenmeyeceğidir.

**Şarta bağlı gem yükleme**

Bazen sadece belli bir şart doğru ise gem'in yüklenmesini isteyebilirsiniz. Mesela sadece bir uygulama sisteminize varsa gem yüklenebilmelidir.

Bu metod ``proc`` ya da ``lambda`` kabul eder. Bu örnekte sadece Mac üzerinde çalışıldığında gem yüklecektir.

{% highlight ruby %}
install_if -> { RUBY_PLATFORM =~ /darwin/ } do
  gem "my_osx_gem"
end
{% endhighlight %}

**Finito**

Okuduğunuz için teşekkürler, umarım yardımcı olmuştur.

> Bu benim ilk makale çevirim. Uzun zamandır bir blog kurma düşüncem vardı. Nihayet gerçekleşti. Düşüncelerinizi twitter üzerinden duymak isterim. Teşekkürler.

**Yazar ve Kaynak**

I would like to thank [Toby Osbourn][twitter-toby] for letting me to translate amazing blog post. Original blog post: [What is a Gemfile][what-is-the-gemfile]

[twitter-toby]: https://twitter.com/tosbourn
[what-is-the-gemfile]: http://tosbourn.com/what-is-the-gemfile/
