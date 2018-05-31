---
layout: post
title: Rails'de model IDleri için UUID'leri seçin
date: 2018-05-31 18:18 +0300
---

Bir [evrensel benzersiz tanımlayıcı](https://en.wikipedia.org/wiki/Universally_unique_identifier) (UUID) bilgisayar sistemlerinde bilgiyi tanımlamak için kullanılan 128 bit uzunluğunda bir sayı dizisidir. Bazen küresel benzersiz tanımlayıcı (GUID) olarak da isimlendirilir.

PostgreSQL içerisinde yerleşik olarak veri tipi bulunur. PostgeSQL yerleşik veri tipleri için [Rails rehberlerinde](http://guides.rubyonrails.org/active_record_postgresql.html) detaylı bilgiye ulaşabilirsiniz.

Rails'in otomatik artan tamsayı id...
# ...yerine.

PostgreSQL'in UUID desteğini...
# ...kullanın.

``Ruby on Rails UUID'leri ActiveRecord modellerinde id olarak kullanma yetisine 5.0 versiyonundan veri sahip.``

***

Öncelikle PostgreSQL eklentisi aktif edin.

``bin/rails g migration enable_extension_for_uuid``

{% highlight ruby %}
  class EnableExtensionForUuid < ActiveRecord::Migration[5.1]
    def change
      enable_extension 'pgcrypto' unless extension_enabled?('pgcrypto')
    end
  end
{% endhighlight %}

``config/initializers/generators.rb`` dosyasını oluşturun.

{% highlight ruby %}
  Rails.application.config.generators do |g|
    g.orm :active_record, primary_key_type: :uuid
  end
{% endhighlight %}  

Bu değişiklik birincil anahtarlarınızın (primary keys) varsayılan veri tipini değiştirecek ve göç (migration) ile oluşturulacak yeni tablolarda ``set id: :uuid`` metodunun kullanılmasını sağlayacaktır.

# Gelecekteki göçlerde...
...ilişki kuracağınız zaman ``type: :uuid`` kullanmak zorundasınız.

{% highlight ruby %}
  class AddNewTable < ActiveRecord::Migration[5.1]
    def change
      create_table :related_model do |t|
        t.references :other, type: :uuid, index: true
      end
    end
  end
{% endhighlight %}  

### Peki neden?

Rails modellerinizde id olarak UUID kullanmanız size çakışma hatalarından kurtulmanıza yardımcı olacaktır. UUID'lerin evrensel olarak benzersiz olması demek başka modellerin aynı idye sahip olamayacağını ve tarayıcı tarafında da başka bir değer atayamayacağınızı bilmeniz demektir.

Artan tamsayı id bilgisi dış dünyadan verinizin büyüklüğünün bilinmesine yol açar. Örneğin 5 numaralı id 5. kaydı temsil eder. UUID ile veri tabanınızdaki tabloların büyüklüğünün bilinmesi imkânsızdır ki bu da bizim gizli tutmak istediğimiz bir bilgidir. Bu sorunu URL'leriniz için halka açık id'ler ya da sahte id'ler (slug) oluşturarak çözebilirsiniz ancak yerleşik bir araç varsa neden kullanmayalım ki?

Güvenlik açısından bakarsak, UUID kullanmak, sadece URL'de kullanılan id kullanılarak yapılan bir tahmin ile kötü niyetli birinin saldırı yaparak veriye ulaşmasını engeller. UUID'lerin tahmin edilmesi inanılmaz zordur.

Bu biraz daha karmaşık bir yolu seçiyorsunuz demektir ama iyi bir amaçla.

### Peki neden olmasın?

Eğer PostgreSQL kullanıyorsanız, bu basit bir değişikliktir ve çok az performans maliyeti vardır. MySQL için ise çok karmışık bir durum ve ben o zahmete girmezdim.

ActiveRecord’un ilk (first) ve son (last) metotları UUID kullanılan id'lerde beklenmeyen bir şekilde çalışıyor. Artık en büyük id'nin en yeni kayıt olduğunu düşünemezsiniz. Bu da kod tabanınıza yabancı geliştiricilerin kafasının karışmasına yol açabilir.

UUID'leri kullanmak yeni projeler için iyi bir fikir ancak çalışan bir sistemi UUID'ye geçirmek için gerçekten iyi bir nedeninizin olması gerekir.

**Yazar ve Kaynak**

I would like to thank [Andy Croll][twitter-andy] for letting me to translate amazing blog post. Original blog post: [Choose UUIDs for model IDs in Rails][choose-uuids-for-model-ids-in-rails]

[twitter-andy]: https://twitter.com/andycroll
[choose-uuids-for-model-ids-in-rails]: https://andycroll.com/ruby/choose-uuids-for-model-ids-in-rails
