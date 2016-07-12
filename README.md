#Git Model Rehberi

Bu model rehberi [*How to Get Your Change Into the Linux
Kernel*](https://www.kernel.org/doc/Documentation/SubmittingPatches),
the [git man pages](http://git-scm.com/doc), ve git topluluklarında popüler olan çeşitli uygulamalardan
esinlenerek hazırlanmıştır.

Farklı dillerdeki çevirileri aşagıda mevcuttur:

- [Chinese (Simplified)](https://github.com/aseaday/git-style-guide)
* [Chinese (Traditional)](https://github.com/JuanitoFatas/git-style-guide)
* [French](https://github.com/pierreroth64/git-style-guide)
* [Greek](https://github.com/grigoria/git-style-guide)
* [Japanese](https://github.com/objectx/git-style-guide)
* [Korean](https://github.com/ikaruce/git-style-guide)
* [Portuguese](https://github.com/guylhermetabosa/git-style-guide)
* [Thai](https://github.com/zondezatera/git-style-guide)
* [Ukrainian](https://github.com/denysdovhan/git-style-guide)

Eger sizde bu projeye katkıda bulunmak isterseniz, projeyi fork edin ve
bir pull request açın.

# İçindekiler

1. [Dallar(Branches)](#dallar)

2. [Commits](#commits)

	1. [Mesajlar](#mesajlar)

3.[Birleştirme(Merging)](#birleştirme)

4.[Misc.](#misc)

## Dallar(Branches)

* *Kisa* ve *açıklayıcı* isimler seçin.
	
  ```shell
  # İyi örnek
  $ git checkout -b oauth-migration
	
  # Kötü örnek - Fazla belirsiz
  $ git checkout -b login\_fix
  ```

* Bir harici servisteki (Örn. Bir Github sorunu) ilgili Ticket
  tanımlayıcılarıda dallar(branch) ismi için uygun adaylardır. Örneğin;
	
  ```shell
  #GitHub Issue #15
  $git checkout -b issue-15
  ```

* Kelimeleri ayırmak için *tire* kullanın.

* Birden fazla kişi aynı özellik üzerinde çalışırken bir tane takım için
  dal , bir tane de kişisel dal yaratılmasi daha elverişli olacaktir.
  Aşagıdaki isim düzenini kullanabilirsiniz:

  ```shell
  $ git checkout -b feature-a/master # takim icin dal
  $ git checkout -b feature-a/maria # Maria’nin kişisel dal’ı
  $ git checkout -b feature-a/nick   # Nick’in kişisel dal’ı
  ```

  Takım için yaratılan dalın kişisel dallar ile birleşmesi
  (bkz.”Merging”). Nihayetinde, takım dalı “master” ile birleşecektir.

* Eger silmemeniz için özel bir neden yok ise, birleşme sağlandıktan
  sonra ana depodan dalınızı siliniz.

  Not: “master” dalında iken birleşmiş dalları listelemek için aşagıdaki
  komutu kullanınız.

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## COMMITS

* Her bir commit de tek bir mantıksal degişiklik olmali. Bir commit’in
  içinde birden fazla *mantıksal değişiklik* yapmayın. Örneğin; eger bir
  yama bir bug’ı düzeltip aynı zamanda performansıda arttırıyor ise, onu
  iki farklı commit şeklinde yazın.

  *Not: Değiştirilmiş dosyalar daki belirli kısımları interaktif bir
  biçimde görüntülemek için* git add -p komutunu kullanınız.

* Tek bir mantıksal değişikliği birden fazla commitlere ayırmayın.
  Örneğin, bir özelliğin uygulaması ve onun testleri aynı commit içinde
  olmalı.

* Erken ve sık aralıklarla commit edin. Küçük ve bagımsız commitlerin
  anlaşılması ve eski haline döndürülmesi bir şeylerin yanlış gitmesi
  durumunda daha kolaydır.

* Commitler mantıksal bir biçimde sıralanmalıdır. Örneğin, eğer *X commiti*
*Y comitinin* içinde yapılan değişiklere baglı ise, *Y commiti* *X
commitinden* önce gelmeli.

Not: *Henüz push edilmemiş local* bir dalda tek başınıza çalışıyor iken,
Commit ile çalışmanızın geçici bir anlık kopyasını almanızda bir sorun
yoktur. Fakat her halükarda dalınızı *push etmeden* once yukarıdakileri
uygulamanız gerekicektir.

### Mesajlar

* Commit mesajı yazarken terminal değil editör kullanın.

  ```shell
  # İyi örnek
  $ git commit

  # Kötü örnek
  $ git commit -m "Quick fix"
  ```

  Terminalden commit etmek herşeyi tek bir komut satırına sıgdırdırmaya
  yönlendiren bir zihniyet oluşturur ki buda genelde bilgilendirici
  olamayan ve belirsiz commit mesajların ortaya çıkmasına neden olur.

* Özet satırı (mesajın ilk satırı) *açıklayıcı* ve aynı zamanda *kısa*
  olmalıdır. İdeali bu satırın *50 karakterden* fazla olmamasıdır. Aynı
  zamanda büyük harf ile başlanmalı şimdiki zaman ve emir kipi
  kullanılmalıdır.

  ```shell
  # İyi Örnek – Şimdiki zaman, Emir kipi, Büyük harf, 50 karakterden az

  Mark huge records as obsolote when clearing hinting faults

  # Kötü Örnek

  fixed ActiveModel :: Errors deprecation messages failing when AR was
  used outside of Rails.
  ```

* Özet satırından sonra bir satır boşluk ve ardından daha ayrıntılı bir
  açiklama gelmelidir. Bu açıklama *72 karaktere* sıgdırılmalı,
  değişikliklere *neden* ihtiyaç duyulduğu açıklanmalı, problemin nasıl
  çözüldüğü ve ne gibi y*an etkileri* olabileceğide belirtilmelidir.

  Ayrıca kullanılan kaynağın linkide verilmelidir. (Örn. Bug takipçisinin
  içindeki ilgili soruna link verilmesi )

  ```text
  Değişimlerin kısa özeti (50 karakterden az)

  Gerekirse daha açiklayıcı bir metin. 72 karaktere sıgdırın. Bazi
  durumlarda ilk satir emailin konusu olarak düşünülür ve geri kalanıda
  metnin gövdesi kabul edilir. Özet ile ana metni birbirinden ayıran boş
  satır çok önemlidir. (Ana metnin tamamını çıkarmadıgınız sürece); özet
  ile ana metin beraber olursa rebase gibi araçlar calıştırıldıgında hata
  ile karşılaşılabilir.

  Sonraki paragraflar boş satırdan sonra gelir.

  - Madde işaretli format kullanılmasında sakınca yoktur

  - Maddelerken tire veya yıldız işareti kullanın sonrasında bir boşluk
  bırakıp cümlenize devam edin yeni maddeye geçerken bir satır boşluk
  bırakın.

  Kaynak
  *http://tbaggery.com.2008/04/19/a-note-about-git-commit-messages.html*
  ```

  Sonuç olarak, bir commit mesajı yazarken bu mesaj ile yıllar sonra
  tekrar karşılaştıgınızda o commit ile ilgili neyi bilmeniz gerektigini
  düşünerek yazın.

* Eger *commit A*, *B commitine* baglı ise, o baglılık *commit A* mesajının
  içinde belirtilmelidir. (Bu işlemler sırasında SHA1(Secure Hash
  Algorithm 1) kullanın)

  Aynı şekilde , *A commiti* *B commiti* tarafından tanıtılan bir bug’ı çözerse,
  bunun mesajı *A commitinin* içerisinde yer almalıdır.

* Eger bir commit başka bir commite sıkıştırılacaksa, yapmak
  istediğinizi düzgün bir biçimde belirtmek için --squash ve –fixup
  bayraklarını sırasıyla kullanınız.

  ```shell
  $ git commit --squash f387cab2
  ```

  *Not: Rebase aracını kullanırken –autosquash bayragını kullanın.
  İşaretlenmiş commitler otomatik olarak sıkıştırılacaktir.*

## Birleştirme(Merging)

* **Yayın tarihini yeniden yazmayınız.** Bir deponun tarihinin kendi içinde
degeri vardır ve o tarihte *tam olarak ne olduğunu* görebilmek çok önemlidir. Yayın
tarihinde değişiklik yapmak projede yer alan herkes icin sorunların
ortak kaynağıdır.

* Ancak bazı durumlarda tarihi yeniden yazmak uygundur. Bunlar:

  * Dal üzerinde bir tek siz calışıyorsanız ve kimse tarafından
    incelenmiyorsa.

  * Dalınızı düzenlemek istiyorsaniz (Örn: commitleri sıkıştırarak) yada
    rebase komutunu kullanarak daha sonra birleştirmek üzere “master” a
    yollarken.

  Kısacası Üretim veya CI serverlarının da dediği gibi “master. üzerindeki
  tarihi veya herhangi başka özel bir dalın tarihini asla değiştirmeyin.

* Tarihi *temiz* ve *sade* tutun. *Birleştirme işlemini yapmadan önce:*

    a) Model rehberine uygun olduğundan emin olun, eğer degil ise uygun hale
    getiriniz (squash/reorder commits,mesajları yeniden yazarak etc.)

    b) Birleştirileceği dala Rebase edin.

	  ```shell
      [my-branch] $ git fetch
      [my-branch] $ git rebase origin/master
      # sonra birleştirin
      ```

      Böylelikle dalımızı “master” dalının en sonuna direk olarak
      ekleyebiliriz buda çok sade bir geçmişin ortaya çıkmasını sağlayacaktır.

      *(Not: Bu strateji kısa vadeli dallardan oluşan projeler icin daha
      uygundur. Bazi zamanlarda rebase komutu yerine merge komutunu kullanmak
      daha iyi olabilir. )*

* Eğer dalınız birden fazla commit içeriyor ise, fast-forward (ff) ile
birleştirme yapmayınız.

  ```shell
  # İyi örnek – birleşme commitinin oluşturulduğundan emin olur

  $ git merge --no-ff my-branch

  #Kötü örnek

  $ git merge my-branch
  ```
  
## Misc.

* Birçok çeşit iş akışı vardır ve her birinin zayıf ve güçlü yönleri
  vardır. Bir iş akışının size uyması, takımınıza, projenize ve geliştirme
  prosedürlerinize bağlıdır.

  Kendinize uygun iş akışını *seçmeniz* ve ona bağlı kalmanız oldukça
  önemlidir.

* *Tutarlı ve istikrarlı olun.* Bu sizin iş akışınızda sahip olmanız gereken
  bir özellik olsada, commit mesajları, dal isimleri etiketler içinde
  geçerlidir. Deponuzun tamamında tutarlı bir modele sahip olmak sadece
  loglara bakarak neyin ne olduğunu anlamanız kolaylaşacaktır. Bir commit
  mesaji vs.

* *Pushlamadan önce test ediniz.* Yarım kalmış bir işi pushlamayınız.

* Sürümleri veya geçmişteki önemli noktaları işaretlemek için [annotated
  tags (ek aciklamali etiketler)](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags) kullanın. Kişisel kullanımlar için
  [lightweight tags (sade etiketler)](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags) kullanın, örneğin ileride referans
  olarak kullanacağınız yer imi commiti gibi.

* Depolarınızın her daim iyi durumda olmasını istiyorsanız, ara sıra
  bakım yapınız.

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# Lisans

![cc license](http://i.creativecommons.org/l/by/4.0/88x31.png)

Bu çalışma [Creative Commons Attribution 4.0
International license](https://creativecommons.org/licenses/by/4.0/)
tarafından lisanslanmıştır.

# Credits

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io
... and [contributors](https://github.com/agis-/git-style-guide/graphs/contributors)!

# Çeviri

- Cüneyt Şentürk / [@CyneytSenturk](https://twitter.com/CyneytSenturk)
- Mert Can Çam   / [@mertcancamm](https://twitter.com/mertcancamm)
