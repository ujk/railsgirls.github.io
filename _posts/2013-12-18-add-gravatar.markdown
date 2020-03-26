---
layout: default
title: Uygulamanıza Gravatar ekleyin
permalink: gravatar
---

# Uygulamanıza Gravatar ekleyin

*Created by Catherine Jones*

Bu klavuz Rails Girls [uygulama geliştirme klavuzunu](http://guides.railsgirls.com/app/) bitirdiğinizi ve [Devise](http://guides.railsgirls.com/devise/) kullanarak yetkilendirme eklemesini bitirdiğinizi kabul eder.

### Önemli

Bu çalışma için Gravatar'da kayıtlı bir e-mail adresiniz olması gerekir. Eğer hala yoksa [gravatar.com](http://en.gravatar.com/) adresine yeni bir kayıt açın.

## *1.* Gravtastic gem'i ekleyin

Gemfile'ı açın `devise` gem altına şunu ekleyin

{% highlight ruby %}
gem 'gravtastic'
{% endhighlight %}

Sonra konsolda

{% highlight sh %}
bundle install
{% endhighlight %}

Bu gravtastic gem'i kuracaktır. Arkasından Rails server'ı tekrar başlatmayı unutmayın.

## *2.* Uygulamanızda Gravatar ekleyin

`app/models/user.rb` dosyasını açın ve ilk satırın altına şu satırları ekleyin

{% highlight ruby %}
include Gravtastic
gravtastic
{% endhighlight %}

## *3.* Gravatar'ı ayarlayın

`app/views/layouts/application.html.erb` dosyasını açın ve 

{% highlight erb %}
<% if user_signed_in? %>
{% endhighlight %}

bloğu içinde fakat 

{% highlight erb %}
<% else %>
{% endhighlight %}

öncesine şunu ekleyin

{% highlight erb %}
<%= image_tag current_user.gravatar_url, :class => "gravatar" %>
{% endhighlight %}

Ve `app/assets/stylesheets/application.css` dosyasının en altına şunları ekleyin:

{% highlight css %}
.gravatar {
  height: 30px;
  width: auto;
}
{% endhighlight %}

Şimdi tarayıcıda sayfayı yenileyin ve gravatar'ı olan bir e-mail ile oturum açtığınızda Gravatar resminizi sağ üst köşede görebilirsiniz.

{% include other-guides.md page="gravatar" %}
