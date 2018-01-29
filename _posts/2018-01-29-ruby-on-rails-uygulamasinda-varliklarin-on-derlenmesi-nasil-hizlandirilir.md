---
layout: post
title: Ruby on Rails uygulamasında varlıkların ön derlenmesi nasıl hızlandırılır?
comments: true
---
Her Ruby on Rails geliştiricisi varlıkların yüklenmesinin gerçekten çok zaman alan bir işlem olduğunu bilir. İşte size bu işlemi hızlandırmak için kullanışlı bazı ipuçları.

# İpuçları

- Her şeyi tek bir devasa dosya içerisine dahil etmeyin. ``CKeditor`` gibi araçlar için CDN kullanın ve sadece ihtiyacınız olan sayfalara ekleyin.

- ``I18n-js`` çevirileri farklı dosyalarda tutar. Sadece ihtiyacınız olan dilleri ayarlayın. Aksi takdirde her yeni eklenen kütüphane ile çeviri dosyaları hızlıca büyüyecektir. Bunu yaparsanız büyük dosya derlemek zorunda kalmayacak dolayısıyla derleme ve yükleme süreleri de kısalacaktır.
{% highlight ruby %}
config.i18n.available_locales = %i(en)
{% endhighlight %}

- [therubyracer][therubyracer] gem'ini Gemfile'dan kaldırın. Bu gem yüksek miktarda ram kullanıyor. Bunun yerine son sürüm Node.js'in yüklü olduğundan emin olun.
    
- SASS/SCSS dosyalarınızda ``require``, ``require_tree`` ve ``require_self`` direktiflerini kullanmayın. Bu direktifler çok ilkeller ve SASS dosyalarında iyi çalışmıyorlar. Bunun yerinde SASS'ın kendi ``@import`` direktifini kullanın. [sass-rails][sass-rails] gem'i Ruby on Rails projelerinde bu kuralın uygulanması konusunda güncellenmiştir.

- SASS/SCSS dosyalarınızda ``@import`` direktifini dikkatli kullanın. Sadece flexbox için ``@import 'compass/css3/flexbox'`` direktifi yeterli iken tüm compass ``@import 'compass'`` paketini dahil etmeyin.

- JS/COFFEE dosyalarınızda ``require_tree .`` direktifini kullanmayın. Örneğin; varlıkları tamamen ayrı uygulamanız ve admin paneliniz olduğunu düşünün.

{% highlight ruby %}
//** assets/javascripts/admin/admin.js

//= require admin/tab.js
//= ...
{% endhighlight %}

{% highlight ruby %}
//** assets/javascripts/application.js

//= require 'something'
//= require_tree . // KÖTÜ! ADMİN VARLIKLARINI DA DAHİL EDİYOR.
{% endhighlight %}    

- Varlıklarınız derlenirken kayıtları kontrol edin. Varsayılan kayıt ediciyi ezmek ve DEBUG modda arka planda neler olduğunu görmek oldukça kolaydır.

{% highlight ruby %}
# /lib/tasks/assets.rake

require 'sprockets/rails/task'

Sprockets::Rails::Task.new(Rails.application) do |t|
  t.logger = Logger.new(STDOUT)
end
{% endhighlight %} 

# Özet

Yukarıdaki ipuçları varlıklarınızın daha hızlı derlenmesinde size fayda sağlayacaktır. Kavram kanıtı olarak [sprockets-tips][sprockets-tips] reposundaki örnek Ruby on Rails projesini inceleyerek daha fazla detaya ulaşabilirsiniz.

Ruby on Rails 5.1 ile varlıkları yönetmenin artık farklı yolları da var. Yarn ile Webpack'i kesinlikle incelemenizi öneririm. Varlık bağımlılık yönetimi, çok daha verimli derleme işlemi, kod değişiminde anında sayfa yenileme, ES6 desteği, PostCSS ve daha fazlasını elde edeceksiniz.

**Yazar ve Kaynak**

Original blog post: [How to Speed Up Assets Precompilation Time in Ruby on Rails App][speed-up-assets-precompilation-time-in-a-rails-app]

[therubyracer]: https://devcenter.heroku.com/articles/rails-asset-pipeline#therubyracer
[sass-rails]: https://github.com/rails/sass-rails#important-note
[sprockets-tips]: https://github.com/cintrzyk/sprockets-tips
[speed-up-assets-precompilation-time-in-a-rails-app]: https://www.netguru.co/codestories/speed-up-assets-precompilation-time-in-a-rails-app