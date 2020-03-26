---
layout: default
title: Fikirleri gösterirken thumbnail kullanmak
permalink: thumbnails
---

# Carrierwave ile thumbnail'ler üretmek

*Created by Miha Filej, [@mfilej](https://twitter.com/mfilej)*

__Eğitmen__: Adım 4 teki HTML içinde resim genişlik özelliğini anlatın ve resimi server'da yeniden boyutlandırmadan farkını anlatın.

## *1.* ImageMagick kurulumu

* macOS: `brew install imagemagick` komutu çalıştırın. "brew" komutunuz yoksa [buradan Homebrew kurabilirsiniz][in-homebrew].
* Windows: [ImageMagick installer][im-win] indirin ve kurun (ilk *download* linkini kullanın). Kurulum sihirbazında "install legacy binaries" seçmeyi unutmayın.
* Linux: Ubuntu ve Debian'da, `sudo apt-get install imagemagick` çalıştırın. Diğer dağıtımlar için dağıtıma ait `apt-get` komutunu kullanın.

  [im-win]: http://www.imagemagick.org/script/download.php#windows
  [in-homebrew]: https://brew.sh/

__Eğitmen__: ImageMagick nedir daha önce kurduğumuz kütühane ve gemlerden farkı nedir açıklayın

Projedeki `Gemfile` açın ve şunu ekleyin

{% highlight ruby %}
gem 'mini_magick'
{% endhighlight %}

şu satırın altına ekleyin

{% highlight ruby %}
gem 'carrierwave'
{% endhighlight %}

Konsolda şunu çalıştırın:

{% highlight sh %}
bundle
{% endhighlight %}

## *2.* Uygulamamıza bir resim yüklendiğinde thumbnail yapmasını gösterelim

`app/uploaders/picture_uploader.rb` dosyasını açın ve şuna benzeyen satırı bulun:

{% highlight ruby %}
  # include CarrierWave::MiniMagick
{% endhighlight %}

`#` işaretini silin.

__Eğitmen__: Kod içindeki yorumların nasıl yapıldığını açıklayın.

Az önce değiştirdiğiniz satırın altına şunu ekleyin:

{% highlight ruby %}
version :thumb do
  process :resize_to_fit => [50, 50]
end
{% endhighlight %}

Şimdiden sonra yüklenen resimler yeniden boyutlandırılacaktır, fakat şu ana kadar yüklenenler hala aynı duruyor. Bu resimleri düzenleyip tekrar yükleyelim.

## *3.* Thumbnail olarak göstermek

Yeni yüklenen resimin boyutlandırıldığını kontrol için `app/views/ideas/index.html.erb` dosyasında şu satırı değiştirin.

{% highlight erb %}
<%= image_tag idea.picture_url, width: '100%' if idea.picture.present? %>
{% endhighlight %}

şöyle yapın

{% highlight erb %}
<%= image_tag idea.picture_url(:thumb) if idea.picture.present? %>
{% endhighlight %}

Şimdi tarayıcıda fikirler listesine gidip bakın thumbnail resimler gelmiş mi?

{% include other-guides.md page="thumbnails" %}
