# Microsoft Power BI

Power BI, Microsoft’un bulut tabanlı iş zekası ve iş analitiği platformudur. Birbirinden bağımsız veri kaynaklarının tutarlı bir şekilde görsellere dönüştürülerek etkili öngörüler elde etmemizi sağlar. Ayrıca veri kaynaklarına kolayca bağlanmamızı, önemli noktaları görselleştirmemizi ve keşfetmemizi, bunu herkesle veya istediğimiz kişilerle paylaşabilmemizi sağlar.


### Power BI ile Neler Yapabiliriz?
- Birden çok veri kaynağı da dahil olmak üzere verilere bağlanma.
- Sorgularla verileri şekillendirme.
- Görselleştirmeleri ve raporları oluşturmak için veri modellerini kullanma.
- Başkalarının yararlanması, temel alması ve paylaşması için raporları paylaşma.

**DAX(Data Analysis Expression)**<br>
  **1-Calculated Column:**<br>
Pivot table da satır,sutun,filtre alanına sürükle bırak yapabileceğimiz,oluşturduğunuz zaman pivot table alan listesinde gözüken formüllerdir.
- Bir tarihin ay değeri
- Bir satışın tutarı
- Exceldeki yardımcı sütun
- Tablo da add column alanından import edilir.

  - ``` #Toplam =SUMX(satis,satis[Adet]*(1-satis[indirim])*RELATED(urun[Fiyat])) ```
 <br>sumx fonksiyonu vermiş olduğumuz tablodaki  her bir  değeri hesaplayarak oluşacak yeni tabloda geçici olarak döndürür

  - ```  #Ortalama =AVERAGEX(satis,satis[Adet]*(1-satis[indirim])*RELATED(urun[Fiyat]))```

  - ```  İşlem_Sayısı = COUNTAX(satis,satis[Satış Temsilcisi İsmi]) ```

**2-Measured(Calculated )Field:**<br>
Pivot table da value alanına bırakacağınız ,herhangi bir sütunun hesaplanması için kullanılır.
- Related,sumx,calculate vb.
- Bir kere hesaplanır.
- Tablonun altında oluşturulur.

  - ``` Toplam Tutar = SUM(satis[Toplam]) ```
  - ``` Satış Temsilcisi İsmi = RELATED(temsilci[Satış Temsilcisi]) ```
  - ``` Haftanın Durumu = IF( WEEKDAY(satis[Tarih],2)>5,"Haftaiçi","Hafta Sonu")```
  - ``` Toplam = satis[Adet]*(1-satis[indirim])*RELATED(urun[Fiyat]) ```
  - ``` Toplam Satış Adedi = SUM(satis[Adet] ) ```
  -  ``` Ortalama ürün fiyatı = AVERAGE(urun[Fiyat]) ```
  - ``` En Yüksek Satıs = MAX(satis[Toplam]) ```

**Saydırma Fonksiyonları**

- COUNT,COUNTA,COUNTBLANK,COUNTROWS,COUNT gibi.
  - ``` Satıs Sayısı = COUNT(satis[Adet]) ```
  - ``` Tüm Satırlar = COUNTA(satis[Fiyat])``` Eğer boş satır varsa onuda saydırır.
  - ``` Boş Hücreleri Say = COUNTBLANK(satis[Haftanın Durumu] )```
  - ``` Benzersiz Sayısı = DISTINCTCOUNT(satis[Satış Temsilcisi İsmi])```


**Calculate() Fonksiyonu** 
- Power Bi filtre kritirene müdahele edip yalnızca sizin verdiğiniz kritirlere göre hesaplama işlemi yapar.
  
  - ``` Marmara + Ege = Calculate([#Toplam],bolge[Bölge] IN {"Marmara","Ege"}) ```   OR fonksiyonu
  - ``` Genel Toplam = CALCULATE([#Toplam],ALL(satis))   ```                         Tablodaki tüm satırları kullanarak hesaplama yapar.
  - ``` Yüzdelik Satıs = [#Toplam]/[Genel Toplam] ```

  - ``` Secili Deger = SELECTEDVALUE(urun[Ürün Adı],"Ürünler")    ```                Ürün adına göre başlık değişiyor.

  - ``` Başlık 3 = IF(HASONEFILTER(urun[Ürün Adı]),VALUES(urun[Ürün Adı]),"Ürünler")```   Ürün adına göre başlık değişiyor.


  - ``` Filter1 = FILTER(satis,satis[Haftanın Durumu]= "Haftasonu")           ```         Satış tablosunu haftasonu olacak şekilde döndürür.

  - ``` 500 Uzerinde Kaç Satıs Var = CALCULATE(COUNTROWS(satis),FILTER(satis,[#Toplam]>500))```

**Takvim Tablosu**
- Örnekler
  - ``` Takvim_Tablosu = CALENDAR(MIN(satis[Tarih]),DATE(YEAR(TODAY()),12,31))```
  - ``` Takvim_Tablosu = CALENDARAUTO()```
  - ``` Year = year(Takvim_Tablosu[Date])```
  - ``` Month = MONTH(Takvim_Tablosu[Date]) ```
  - ``` Month_Name = format(Takvim_Tablosu[Date],"mmmm")```  format() ,ayları text sutununa çevirir
  - ``` Quarter = "Q-"&FORMAT(Takvim_Tablosu[Date],"Q") ```  such as : Q-1,Q-2..
  - ``` Week = "W-"&WEEKNUM(Takvim_Tablosu[Date],2)   ```    such as : W-1,W-2..
  - ``` Day_Name = FORMAT(Takvim_Tablosu[Date],"dddd")```
  - ``` Önceki Yıl = CALCULATE([#Toplam],SAMEPERIODLASTYEAR(Takvim_Tablosu[Date]))```    Takvim tablosu döndürür
  - ``` % Büyüme = DIVIDE([#Toplam],[Önceki Yıl])-1```
  - ``` OncekiAy = CALCULATE([#Toplam],DATEADD(Takvim_Tablosu[Date],-1,MONTH))  ```    DATEADD() fonksiyonu ile vermiş olduğumuz zaman diliminde ,geriye veya ileriye gidilebilir.
  - ``` ParelelPeriod = CALCULATE([#Toplam],PARALLELPERIOD(Takvim_Tablosu[Date],-1,YEAR))```
  - ``` %lık GecenYıl = [Önceki Yıl]/[ParelelPeriod]```
  - ``` TotalYeartoDate = TOTALYTD([#Toplam],Takvim_Tablosu[Date])```
  - ``` TotalMTD = TOTALMTD([#Toplam],Takvim_Tablosu[Date])```
  - ``` TotalQTD = TOTALQTD([#Toplam],Takvim_Tablosu[Date])```
