Bu depo, Siemens TIA Portal ile geliştirilmiş, farklı endüstriyel otomasyon senaryolarını içeren bir dizi bağımsız projeyi barındırır. Projeler, temel sensör verisi işlemeden motor kontrolüne, HMI ekran tasarımlarından güvenlik (safety) uygulamalarına kadar geniş bir yelpazeyi kapsar. Bu, temel PLC programlama mantıklarını ve HMI arayüzü tasarımlarını uygulamalı olarak öğrenmek için ideal bir koleksiyondur.

Çalışma Mantığı
Bu gruptaki projeler, her biri farklı bir amaca hizmet eden birden fazla bağımsız senaryoyu kapsar. Çalışma mantıkları aşağıda her bir senaryo için ayrı ayrı açıklanmıştır.

1. Analog Sinyal İşleme ve Güvenlik (Safety) Uygulaması
Bu senaryo, bir sensörden gelen analog sinyali okumayı ve güvenlik donanımını yapılandırmayı gösterir.

Analog Değer İşleme: NORM_X ve SCALE_X fonksiyon blokları, bir önceki projedeki gibi çalışır. NORM_X (%MW100 - AnalogGiris) ile ham analog sinyal normalize edilir, ardından SCALE_X (%MD102) ile yüzde olarak ölçeklendirilir.

Güvenlik Programı: Proje, bir F-CPU (Fail-safe CPU) içerir ve Main_Safety_RTG1 gibi özel güvenlik blokları ve bir veri bloğu (Main_Safety_RTG1_08 [DB1]) kullanır. Bu, potansiyel tehlikeli durumlarda sistemin güvenli bir duruma getirilmesi için gerekli olan emniyetli fonksiyonların programlandığını gösterir.

HMI Görselleştirme: Root screen, bir PT100 sensörü ve bir SITRANS TH100 transmitteri görselleştirir. PLC'den gelen analog değer (0-27648) bir çubuk grafik ve sayısal gösterge ile operatöre sunulur.

2. Motor Yönü Kontrolü (İleri / Geri)
Bu senaryo, bir motorun saat yönünde (CW) ve saat yönünün tersine (CCW) hareketini kontrol etmek için tasarlanmıştır.

CW Kontrolü (Network 1):

%I0.0 (CW START) sinyali, SR (Set-Reset) lojik bloğunun S girişini tetikler ve %Q4.0 (CW) çıkışını aktif eder. Bu, motoru saat yönünde çalıştırır.

%I0.2 (STOP) veya %I0.1 (CCW START) sinyali geldiğinde R1 girişi aktif olur ve CW çıkışını sıfırlar. Bu, iki yönlü çalışmada çakışmayı önlemek için bir kilitleme (interlock) mekanizmasıdır.

CCW Kontrolü (Network 2):

%I0.1 (CCW START) sinyali, SR bloğunun S girişini tetikler ve %Q4.1 (CCW) çıkışını aktif eder. Bu, motoru saat yönünün tersine çalıştırır.

%I0.2 (STOP) veya %I0.0 (CW START) sinyali geldiğinde R1 girişi aktif olur ve CCW çıkışını sıfırlar.

Bu SR lojiği, aynı anda hem CW hem de CCW komutlarının verilmesini engeller ve motorun yanlışlıkla iki yönde aynı anda çalışmasını önler.

3. Tek Butonla Kontrol ve Dolum Hattı Uygulaması
Bu senaryo, tek bir butonla başlatma/durdurma mantığını ve bir HMI'da dolum hattı görselleştirmesini bir araya getirir.

Tek Butonla Kontrol: Network 1'de, %I0.0 (START-STOP) ve %M0.0 (Merdivenlama) gibi dahili bitler kullanılarak bir motorun tek bir butonla SET ve RESET edilmesi sağlanır. Her butona basıldığında motorun durumu değişir. Bu mantık, basit uygulamalarda donanım maliyetini düşürmek için kullanılır.

HMI Dolum Hattı: Root screen, bir dolum tesisini görselleştirir. Üç farklı renkli tank, bir karıştırıcı, bir konveyör bandı ve dolum valfleri bulunur. HMI'daki butonlar (LATCHED, MOMENTARY, STOP), PLC'deki %I0.0, %I0.1 ve %I0.2 girişlerine bağlanır. Bu, operatörün sistemi kolayca kontrol etmesini sağlar. MOTOR çıkışı (%Q4.0) ise konveyör motorunu çalıştırır.
