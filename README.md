# Reddit Platformu Üzerinden Bilimle İlgili Gönderilerin İlişkili Konu Modelleme Analizi ile Bilim Dünyasının Haritasının Çıkarılması
<h2>Correlated Topic Model(CTM)</h2>
Geliştirilmiş bir yöntem olan ilişkili konu modelleme (İKM), belgelerdeki gizli konular arasındaki ilişkiyi inceleyen bir modeldir. Geleneksel GDA yönteminde, Dirichlet dağılımları kullanıldığı için konular arasındaki ilişki göz ardı edilir. Ancak İKM'de, Dirichlet dağılımlarının yerine lojistik normal dağılımlar kullanıldığı için gizli konular arasındaki korelasyon daha iyi ortaya konulabilir. Bu, farklı anlamların birbirleriyle ilişkilendirilerek daha gerçekçi bir model sunulmasını sağlar. Ayrıca bilimsel çalışmalar, İKM'nin GDA'ya göre daha iyi sonuçlar verdiğini göstermiştir.

Bir örnek olarak, Blei ve diğerleri tarafından yürütülen bir çalışmada, 1990-1999 yıllarına ait Science dergisindeki makalelere İKM uygulanmış ve GDA'ya göre daha iyi sonuçlar elde edilmiştir. İKM, yapılandırılmamış belgeleri daha iyi görselleştirmek ve anlamlandırmak için kullanışlı bir yöntemdir.

İKM'de her belge farklı oranlarda birden fazla konuyu içerebilir. Bu, gruplanmış verilerde çeşitliliğin yakalanmasına olanak tanır. İKM'nin veri, gizli değişkenler ve parametreleri tanımlamak için kullandığı grafiksel gösterim aşağıda verilmiştir.

<img src="https://github.com/semanurgursoy/NLP_CTM_Project/blob/main/ReadMeImages/model_graf.png" alt="ctm graf" width="600" height="300">

İKM’nin Şekil 1’deki grafiksel gösterimine karşılık gelen üretici modeli aşağıdaki gibidir:
<ol>
  <li>
      Her konu k için k ϵ K 
    <ol type="a">
      <li>
        Kelimelerin konular içerisindeki K çok terimli dağılımını belirle: βk ~ N (μ,Σ)
      </li>
    </ol>
  </li>
  <li>
      Her doküman d için d ϵ D 
    <ol type="a">
      <li>
        K-boyutlu vektörü yani her belge için konu dağılımını belirle: ηd ~ N (μ,Σ) 
      </li>
      <li>
        Her kelime n için n ϵ {1, … ,Nd} 
      </li>
      <ol type="I">
        <li>
          Örnek bir konu ataması yap: Zd,n ~ Mult(θ)
        </li>
        <li>
          Örnek bir kelime seç: Wd,n ~ Mult(βZd,n) 
        </li>
      </ol>
    </ol>
  </li>
</ol>
Grafiksel gösterim ve üretici modelde belirtilen parametreler ve bu parametrelere karşılık gelen anlamlar ise aşağıdaki gibidir.

<img src="https://github.com/semanurgursoy/NLP_CTM_Project/blob/main/ReadMeImages/model_means.png" alt="ctm param means" width="700" height="500">

<h2>Metot</h2>
<h3>Verilerin Çekilmesi</h3>
Academic Torrents üzerinden Reddit websitesinin 2022 yılının ilk 9 ayına ait gönderiler elde edilip bilim başlığı altındaki gönderiler çekilmiştir. Her bir ay için elde edilen gönderi verileri, o gönderiye ait olan yorumlarla birlikte maksimum 10.000 kelime içerecek şekilde birleştirilmiştir. 8,456,995 kelimeden oluşan 21400 adet veriye sahip veri seti csv formatında kaydedilmiştir.
<h3>Ön İşleme Aşamalarının Gerçekleştirilmesi</h3>
Konu modelleme için gerekli temel ön işleme adımları gerçekleştirilmiştir: istenmeyen içeriğin silinmesi, durak kelimelerin kaldırılması ve kök sıkılama.
<h3>İKM Parametrelerinin Belirlenmesi</h3>
İKM modelleri oluşturulurken Pythonda Tomotopy kütüphanesi kullanılmıştır. İKM’yi oluştururken birçok parametre kullanılır. 
<ul>
  <li>
    k, konuların sayısını ifade eder, 1 ile 32.767 arasında değer alır. Bu değer gerçekleştirilen tutarlılık testlerine göre seçilmiştir. 
  </li>
  <li>
    min_df, kelimelerin minimum belge sıklığıdır. Belge sıklığı bu değerden küçük olan sözcükler modelden çıkarılır. Varsayılan değer 0'dır, yani hiçbir sözcük     hariç tutulmaz. Bu değer 5 olarak belirlendi. 
  </li>
  <li>
    rm_top, çıkarılacak çok yaygın olan kelimelerin sayısıdır. Varsayılan değer 0'dır ve 40 olarak belirlendi. 
  </li>
  <li>
    num_beta_sample, beta parametrelerinin örneklenme sayısıdır, varsayılan değeri 10'dur. CTModel, her belge için num_beta_sample beta parametrelerini örnekler.       Ne kadar çok beta örneklenirse dağılım o kadar doğru olur, ancak öğrenmesi de o kadar uzun sürer. Modelde az sayıda belge varsa bu değeri büyük tutmak daha iyi     sonuç almada yardımcı olur. Yeterli belge sayısı(18461) göz önünde bulundurularak num_beta_sample değeri 5 olarak seçildi. 
  </li>
</ul>
<h3>Tutarlılık Değerinin Belirlenmesi</h3>
Uygun tutarlılık değerlerine sahip modeli bulmak için 5 ve 40 konu sayısı arasında modeller oluşturulup tutarlılık değerleri karşılaştırılmıştır. Bu tutarlılık değerleri aşağıdaki grafikte verilmiştir. İyi bir tutarlılık değerine sahip olan ve konular arası ilişkiyi net bir şekilde ortaya koyan en küçük k değeri 28 olarak belirlenmiştir.<br><br>

<img src="https://github.com/semanurgursoy/NLP_CTM_Project/blob/main/ReadMeImages/coh_graf.png" alt="coherance values" width="500" height="400"><br>

<h2>Sonuçlar</h2>
Bu çalışmada, Reddit platformunda 2022 yılının ilk 9 ayına ait bilimle ilgili 21,400 paylaşımın incelenmesi amaçlanmıştır. Her bir paylaşım için en fazla 10,000 kelime kullanılarak konu modelleme yapılmıştır. Veriler öncelikle ön işleme adımlarından geçirilmiş ve ardından İKM modeli kullanılarak paylaşımlardaki konular belirlenmiş ve bu konular arasındaki ilişkiler analiz edilmiştir. Elde edilen sonuçlar görselleştirilmiştir.<br><br>

Konular arası mesafe haritası (intertopic distance map), konuların iki boyutlu bir alanda görselleştirilmesine dayanır. Görselde konuları temsil eden dairelerin alanı, konuya ait olan kelime miktarıyla doğru orantılıdır. Ayrıca birbirine daha yakın olan konular daha çok ortak kelimeye sahiptir. Konular arası mesafe haritası aşağıda verilmiştir. Görselde örneğin konu-15 ile konu-18'in ortak kelimelere sahip olduğu görülmektedir.

<img src="https://github.com/semanurgursoy/NLP_CTM_Project/blob/main/ReadMeImages/k_28_dist.png" alt="distribution" width="600" height="450"><br>

Konu dağılımlarına ait bar dağılım grafı aşağıda verilmiştir.

<img src="https://github.com/semanurgursoy/NLP_CTM_Project/blob/main/ReadMeImages/k_28_bar-graf.png" alt="bar graf for topic rates" width="800" height="400"><br>

Konular arası ilişkileri ortaya koyan görsel aşağıda görüldüğü gibidir. Hemen hemen tüm konular arasında ilişki mevcuttur. Fakat tüm ilişkiler şekil üzerinde gösterildiğinde şekli anlamak zorlaştığı için 0,46 eşik değerinin üzerinde kalan ilişkiler gösterilmiştir. Konular arasındaki kalın bağ güçlü ilişkileri gösterirken ince bağ nispeten daha zayıf ilişkileri göstermektedir.

<img src="https://github.com/semanurgursoy/NLP_CTM_Project/blob/main/ReadMeImages/k_28_graf.png" alt="corr graf" width="500" height="450"><br>

Konular arası ilişkiler sayısal değerlerle de aşağıdaki tabloda verilmiştir. Tabloda pozitif değerler konular arasında pozitif yönde bir ilişki olduğunu gösterirken negatif değerler ise konular arasında zıt yönde bir ilişki olduğunu göstermektedir. Tabloya bakıldığında konunun kendisiyle olan ilişkisinin 1 olarak ifade edildiği görülmektedir.

<img src="https://github.com/semanurgursoy/NLP_CTM_Project/blob/main/ReadMeImages/k_28_corr.png" alt="corr table" width="800" height="600"><br>

 Tabloda örneğin konu-9 ile konu-10 arasında 0.57 oranına sahip bir ilişki vardır. Bu konulara ait popüler kelimeler incelendiğinde her iki konuda da toplumsal kavram ve deneyimlerden bahsedildiği görülür. Bu durum göz önünde bulundurulduğunda bu ilişkinin anlamlı olduğu anlaşılmış olacaktır.<br>
 Konu-9 için en popüler 30 kelime: mask die life hous polic old wear live hit never stay stupid man fine boy girl lockdown day longer offic harm without score back meal stop altern later though crimin<br>
 Konu-10 için en popüler 30 kelime: societi trust experi religi religion physic fear problem play begin often situat without bad term sport done exampl side context sometim point uk tell admit better around interpret task touch
 
