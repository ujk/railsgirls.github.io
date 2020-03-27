---
layout: default
title: Rails Girls Sinatra Klavuzu
permalink: sinatra-app
---

# Sinatra ile ilk oylama uygulamanızı yapın

*Created by Piotr Szotkowski, [@chastell](https://twitter.com/chastell)*

Ruby on Rails'e benzeyen Sinatra isimli Ruby web geliştirme iskeletini kullanarak küçük bir oylama uygulaması yapacağız. İş bitirmek için aynı zamanda eğlenceli bir araç!

Düşünün, arkadaşlarınızla haftasonu yapacağınız film izleme maratonu için neler sipariş edeceğinizi tartışıyorsunuz. Bir çok fast food opsiyonları var, karar vermek zorlaşıyor. Uygulamamız burada devreye giriyor!

__EĞİTMEN__: [Sinatra'nın](http://www.sinatrarb.com) ne olduğunu kısaca anlatın.

## Sinatra Kuruluması

Ruby on Rails'i nasıl kurduğumuzu hatırlayın. Benzer şekilde Sinatra kurmamız gerekiyor:

`gem install sinatra`

### İlk Sinatra uygulamanızı yapın

`suffragist.rb` dosyasını içinde şu kodlarla üretin:

{% highlight ruby %}
require 'sinatra'

get '/' do
  'Merhaba oy veren!'
end
{% endhighlight %}


Ruby dosyanıza istediğiniz ismi verebilirsiniz. Mesela `oylama.rb` de olsa gayet güzel çalışır. Fakat [suffragist](http://www.vocabulary.com/dictionary/suffragist) kadın hakları hareketinde çok önemli bir olayı ifade ediyor, o yüzden bu isimle devam etmeyi seçtik!  


### Uygulamayı çalıştırın

Uygulamayı koyduğunuz klasöre konsolda gidip `ruby suffragist.rb` komutunu çalıştırın.
Şimdi tarayıcıda <a href="localhost:4567" target="_blank">localhost:4567</a> sayfasına gidin. 'Merhaba oy veren!' yazısı olan bir sayfa görmelisiniz, bunun anlamı uygulamanız çalışmaya başlamış demektir. Konsolda <kbd>Ctrl</kbd>+<kbd>C</kbd> basarak server'ı durdurun. (Eğer <kbd>Ctrl</kbd>+<kbd>C</kbd> sizin için çalışmazsa muhtemelen Windows kullanıyorsunuz ve <kbd>Ctrl</kbd>+<kbd>Z</kbd>/ <kbd>Ctrl</kbd>+<kbd>Pause</kbd> / <kbd>Ctrl</kbd>+<kbd>Break</kbd> sorunu çözecektir)

__EĞİTMEN__: POST ve GET metodları ile tarayıcıyla nasıl etkileştiklerini açıklayın.



### index görselini ekleyin

Herşeyi düzenli tutabilmek için görselleri içeren bir klasör oluşturalım (ve adını `views` koyalım).

Bu kodu `views` klasörü altındaki `index.erb` dosyasına yazalım:

{% highlight erb %}
<!DOCTYPE html>
<html>
  <head>
    <meta charset='UTF-8' />
    <title>Suffragist</title>
    <link href='//netdna.bootstrapcdn.com/twitter-bootstrap/2.3.1/css/bootstrap-combined.min.css' rel='stylesheet' />
  </head>
  <body class='container'>
    <p>Akşam ne yiyelim?</p>
    <form action='cast' method='post'>
      <ul class='unstyled'>
        <% Choices.each do |id, text| %>
          <li>
            <label class='radio'>
              <input type='radio' name='vote' value='<%= id %>' id='vote_<%= id %>' />
              <%= text %>
            </label>
          </li>
        <% end %>
      </ul>
      <button type='submit' class='btn btn-primary'>Oyumu Gönder!</button>
    </form>
  </body>
</html>
{% endhighlight %}

Ve `suffragist.rb` dosyası içinde ilk satırın altına:

{% highlight ruby %}
Choices = {
  'HAM' => 'Hamburger',
  'PIZ' => 'Pizza',
  'CUR' => 'Curry',
  'NOO' => 'Noodles',
}
{% endhighlight %}

`get` aksiyonunu da değiştirin:

{% highlight ruby %}
get '/' do
  erb :index
end
{% endhighlight %}

Konsolda `ruby suffragist.rb` ile çalıştırın ve sonucu görün. <kbd>Ctrl</kbd>+<kbd>C</kbd> ile server'ı durdurun.

__EĞİTMEN__: Biraz HTML ve erb hakkında anlatın. Görsel kalıplarını anlatın. Global sabitler hakkında anlatın.



### Template'ler (Görsel kalıpları)

`index.erb` dosyasını düzenleyin ve `<h1>…</h1>` satırı ekleyin:

{% highlight erb %}
  <body class='container'>
    <h1><%= @title %></h1>
    <p>What's for dinner?</p>
{% endhighlight %}

`get` aksiyonu da şöyle değiştirin:

{% highlight ruby %}
get '/' do
  @title = 'Welcome to the Suffragist!'
  erb :index
end
{% endhighlight %}

__EĞİTMEN__: Oluşum değişkenleri ve Sinatra'nın onları görseller içinde kullanış şeklini anlatın.



### Sonuçların POST ile gönderimi ekleyin

`suffragist.rb` içine şunu ekleyin:

{% highlight ruby %}
post '/cast' do
  @title = 'Oy gönderdiğiniz için teşekkürler!'
  @vote  = params['vote']
  erb :cast
end
{% endhighlight %}

Gördüğünüz gibi "cast" adında bir görsel gerekiyor. `views` klasörü altında `cast.erb` dosyasını ekleyin ve içine biraz Ruby kodu katılmış HTML ekleyin:

{% highlight erb %}
<!DOCTYPE html>
<html>
  <head>
    <meta charset='UTF-8' />
    <title>Suffragist</title>
    <link href='//netdna.bootstrapcdn.com/twitter-bootstrap/2.3.1/css/bootstrap-combined.min.css' rel='stylesheet' />
  </head>
  <body class='container'>
    <h1><%= @title %></h1>
    <p>Oyunuz : <%= Choices[@vote] %></p>
    <p><a href='/results'>Sonuçları görün!</a></p>
  </body>
</html>
{% endhighlight %}

__EĞİTMEN__: POST nasıl çalışır anlatın. Form içinde olanları nasıl algılar. `params` nereden geliyor?



### Ortak bir yerleşim ile düzenleyin

`views` klasöründe `layout.erb` dosyasını ekleyin. İçine şu kodu yazın:

{% highlight erb %}
<!DOCTYPE html>
<html>
  <head>
    <meta charset='UTF-8' />
    <title>Suffragist</title>
    <link href='//netdna.bootstrapcdn.com/twitter-bootstrap/2.3.1/css/bootstrap-combined.min.css' rel='stylesheet' />
  </head>
  <body class='container'>
    <h1><%= @title %></h1>
    <%= yield %>
  </body>
</html>
{% endhighlight %}

Yukarıdaki parçayı diğer iki görsel kalıptan çoıkartın (`views` klasöründeki`index.erb` ve `cast.erb` dosyaları).

__EĞİTMEN__: HTML döküman yapısı ve ortak kod dosyalarıyla düzenlemenin nasıl çalıştığını anlatın. `yield` ne yapar açıklayın.



### results route'unu ve results görselini ekleyin

Aşağıdaki kodu `suffragist.rb` içine ekleyin:

{% highlight ruby %}
get '/results' do
  @votes = { 'HAM' => 7, 'PIZ' => 5, 'CUR' => 3 }
  erb :results
end
{% endhighlight %}

`views` klasörü altında `results.erb` isminde bir dosya ekleyin.

{% highlight erb %}
<table class='table table-hover table-striped'>
  <% Choices.each do |id, text| %>
    <tr>
      <th><%= text %></th>
      <td><%= @votes[id] || 0 %>
      <td><%= '#' * (@votes[id] || 0) %></td>
    </tr>
  <% end %>
</table>
<p><a href='/'>Daha çok oy kullanın!</a></p>
{% endhighlight %}

`ruby suffragist.rb` çalıştırın, sonuçları kontrol edin 
<kbd>Ctrl</kbd>+<kbd>C</kbd> ile server'ı durdurun.

__EĞİTMEN__: HTML tablolarını ve hash listesinde olmayan değerin nasıl sıfır olarak
geldiğini anlatın.



### YAML::Store ile sonuçları saklamak

Yeni bir şeyler yapma zamanı! Hadi seçimlerimizi saklayalım.

`suffragist.rb` dosyasının üstüne şunu ekleyin:

{% highlight ruby %}
require 'yaml/store'
{% endhighlight %}

`suffragist.rb` içine bazı kodlar eklenecek – 
`post '/cast'` ve `get '/results'` bloklarını şöyle değiştirin:

{% highlight ruby %}
post '/cast' do
  @title = 'Oy gönderdiğiniz için teşekkürler!'
  @vote  = params['vote']
  @store = YAML::Store.new 'votes.yml'
  @store.transaction do
    @store['votes'] ||= {}
    @store['votes'][@vote] ||= 0
    @store['votes'][@vote] += 1
  end
  erb :cast
end

get '/results' do
  @title = 'Sonuçlar:'
  @store = YAML::Store.new 'votes.yml'
  @votes = @store.transaction { @store['votes'] }
  erb :results
end
{% endhighlight %}

__EĞİTMEN__: YAML nedir anlatın.


### Oylar gönderildikçe YAML dosyasının değişimini takip edin

`votes.yml` dosyasını açın. Bir oy daha gönderin. Tekrar sonuca bakın.

__EĞİTMEN__: Bazı öğrencilerin server'ı kapatmadan yeni bir tane açtıklarına dair 
durumlar çıkacaktır. İnternette araştırmak için güzel bir konu olacaktır.

__EĞİTMEN__: Son olarak Sinatra ve Rails arasındaki farkları anlatın.



## Uygulama ile oynayın

Uygulamada değiştirmek istediğiniz bazı yerlere müdahale edin:

* Görselllere ilave lojik ekleyin.
* Sonuçlara doğrudan yönlendirin.
* Başka bir oylama ekleyin. YAML dosyasının nasıl değişmesi gerekir?
* Dosyayı başka stillerde şekilllendirmeye çalışın.

{% include other-guides.md page="sinatra-app" %}
