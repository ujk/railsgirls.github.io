---
layout: default
title: HTML ve CSS ile Uygulamanıza stil ekleyin
permalink: design-html-css
---

## *1.* header'ınızı düzenleyin

`app/assets/stylesheets/application.css` dosyasının en altına şu kodu ekleyin:

{% highlight css %}
.navbar {
  min-height: 38px !important;
  background-color: #f55e55 !important;
}
{% endhighlight %}

Şimdi sayfayı yenileyip değişimleri kontrol edin. Başlık yazısı rengi ya da fontunu da değiştirebilirsiniz. Renkleri  [http://color.uisdc.com/](http://color.uisdc.com/) adresinde inceleyebilirsiniz.

Sonra da en alta bunları ekleyin：

{% highlight css %}
.navbar a.brand { font-size: 18px; }
.navbar a.brand:hover {
 color: #fff;
 background-color: transparent;
 text-decoration: none;
}
{% endhighlight %}

**Eğitmen:** bir linkin 4 durumunu açıklayın


## *2.* table tag ayarlayın

Twitter [Bootstrap](http://getbootstrap.com/) ile tablo stilimizi belirliyoruz. `app/views/ideas/index.html.erb` dosyasında bu satırı bulun:

{% highlight html %}
<table>
{% endhighlight %}

ve böyle değiştirin

{% highlight html %}
<table class="table">
{% endhighlight %}

Aşağıdaki satırla ile resim boyutunu değiştirin

{% highlight erb %}
<%= image_tag(idea.picture_url, :width => 600) if idea.picture.present? %>
{% endhighlight %}

"width" özelliğinin değerini değiştirerek nasıl etkisi olduğunu deneyin

`app/assets/stylesheets/ideas.css.scss` dosyasının altına şu satırları ekleyin:

{% highlight css %}
.container a:hover {
  color: #f55e55;
  text-decoration: none;
  background-color: rgba(255, 255, 255, 0);
}
{% endhighlight %}

`background-image` özelliği ile arkaplan stili değiştirin, [http://subtlepatterns.com/](http://subtlepatterns.com/) adresinde bazı patern örnekleri mevcut.

## *3.* footer stilini değiştirin

`app/assets/stylesheets/application.css` dosyasının altına şunları ekleyin:

{% highlight css %}
footer {
  background-color: #ebebeb;
  padding: 30px 0;
}
{% endhighlight %}

`footer` stiline başka şeyler de deneyin, sonra pozisyonunu değiştirin.

## *4.* button'a stil ekleyin

[http://localhost:3000/ideas/new](http://localhost:3000/ideas/new) sayfasını açın ve `Create Idea` düğmesini bulun.

`app/assets/stylesheets/ideas.css.scss` dosyasına şunları ekleyin

{% highlight css %}
.container input[type="submit"] {
  height: 30px;
  font-size: 13px;
  background-color: #f55e55;
  border: none;
  color: #fff;
}
{% endhighlight %}

**Eğitmen:** CSS'de `border` nasıl kullanılır açıklayın, butonun köşelerini yuvarlatılmış yapmayı deneyin, gölge ekleyin vs.

{% include other-guides.md page="design-html-css" %}
