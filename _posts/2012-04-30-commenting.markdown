---
layout: default
title: Rails Girls uygulamasına yorum eklenmesi
permalink: commenting
---
# Rails Girls Uygulamanıza Kullanıcı Yorumu Eklemek
*Created by Janika Liiv, [@janikaliiv](https://twitter.com/janikaliiv)*

*railsgirls* uygulamanızda yayınlanan fikirlere yorum yapma özelliği ekleyeceğiz.

Rails kurulumu ve Fikirler uygulamasının yapımı hakkında bilgiyi [burada](/app) bulabilirsiniz.

## *1.* comment scaffold oluşturun

Yorumcunun adı, yorumun içeriği ve ideas tablosunda yorumyapılan fikire bir referans (`idea_id`) içeren bir comment scaffold üretelim.

{% highlight sh %}
rails g scaffold comment user_name:string body:text idea_id:integer
{% endhighlight %}

Bu komut ayrıca comments tablosu üretmek için bir migrasyon dosyası oluşturacaktır. Migrasyonu çalıştırmak için şu komutu girin

{% highlight sh %}
rails db:migrate
{% endhighlight %}

## *2.* Modellere veri bağlantısı ekleyin

Rails'in tablolar arasındaki bağlantıyı bilmesi gerekir (ideas ve comments tabloları).
Bir fikir birçok yoruma sahip olabileceği için idea modelinin bunu bilmesini sağlamalıyız.
`app/models/idea.rb` dosyasını editörde açın ve şu satırın altına

{% highlight ruby %}
class Idea < ApplicationRecord
{% endhighlight %}

şunu ekleyin

{% highlight ruby %}
has_many :comments
{% endhighlight %}

comment modelinin de yorumların bir fikire ait olduğunu bildirmemiz gerekir. `app/models/comment.rb` dosyasını açın ve şu satırın altına

{% highlight ruby %}
class Comment < ApplicationRecord
{% endhighlight %}

şunu ekleyin

{% highlight ruby %}
belongs_to :idea
{% endhighlight %}

## *3.* Yorum formunu ve mevcut yorumları sayfada yayınlayın

`app/views/ideas/show.html.erb` dosyasını açın ve image_tag sonrasına

{% highlight erb %}
<%= image_tag(@idea.picture_url, :width => 600) if @idea.picture.present? %>
{% endhighlight %}

şunları ekleyin

{% highlight erb %}
<h3>Comments</h3>
<% @comments.each do |comment| %>
  <div>
    <strong><%= comment.user_name %></strong>
    <br>
    <p><%= comment.body %></p>
    <p><%= link_to 'Delete', comment_path(comment), method: :delete, data: { confirm: 'Are you sure?' } %></p>
  </div>
<% end %>
<%= render partial: 'comments/form', locals: { comment: @comment } %>
{% endhighlight %}

`app/controllers/ideas_controller.rb` dosyasında show metodu içine şunları ekleyin

{% highlight ruby %}
@comments = @idea.comments.all
@comment = @idea.comments.build
{% endhighlight %}

`app/views/comments/_form.html.erb` dosyasını açın ve şu satırdan sonra

{% highlight erb %}
  <div class="field">
    <%= form.label :body %><br>
    <%= form.text_area :body, id: :comment_body %>
  </div>
{% endhighlight %}

şu satırı ekleyin

{% highlight erb %}
<%= form.hidden_field :idea_id %>
{% endhighlight %}

sonra, şunları silin

{% highlight erb %}
<div class="field">
  <%= form.label :idea_id %>
  <%= form.number_field :idea_id %>
</div>
{% endhighlight %}

Bu kadar. Şimdi tanımladığınız bir fikire show tıklayın ve yorum ekleme formu ile eski yorumların silinmesi için linkleri görün.

{% include other-guides.md page="commenting" %}
