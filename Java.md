# Nihai Java Cheatsheet'i

## İçindekiler

1. [Temel Kavramlar](#temel-kavramlar)
   - [Değişkenler ve Veri Türleri](#değişkenler-ve-veri-türleri)
   - [Final Anahtar Kelimesi](#final-anahtar-kelimesi)
   - [Operatörler](#operatörler)
   - [Tür Dönüşümü](#tür-dönüşümü)
   - [Kapsam](#kapsam)
2. [Kontrol Akışı](#kontrol-akışı)
   - [Koşullu İfadeler](#koşullu-i̇fadeler)
   - [Döngüler](#döngüler)
3. [Metotlar](#metotlar)
4. [Diziler ve Koleksiyonlar](#diziler-ve-koleksiyonlar)
5. [Nesne Yönelimli Programlama](#nesne-yönelimli-programlama)
6. [Hata Yönetimi](#hata-yönetimi)
7. [Eşzamanlılık](#eşzamanlılık)
8. [Modern Java Özellikleri](#modern-java-özellikleri)
   - [Lambda İfadeleri ve Stream API](#lambda-i̇fadeleri-ve-stream-api-detaylı)
   - [Optional Sınıfı](#optional-sınıfı)
   - [Yeni Tarih ve Zaman API'si](#yeni-tarih-ve-zaman-apisi-javatime)
   - [Metin Blokları](#metin-blokları-text-blocks---java-15)
   - [Switch Expressions](#switch-expressions-java-14)
   - [var Anahtar Kelimesi](#var-anahtar-kelimesi-java-10)
   - [Records](#records-java-14)
   - [Sealed Classes](#sealed-classes-java-17)
9. [Generics](#generics)
10. [Annotations](#annotations)
11. [Enum Türleri](#enum-türleri)
12. [File I/O ve NIO](#file-io-ve-nio)
13. [Inner Classes](#inner-classes)  
14. [equals, hashCode ve Comparisons](#equals-hashcode-ve-comparisons)
15. [Regular Expressions](#regular-expressions)
16. [System ve JVM](#system-ve-jvm)
17. [Paketler ve Sık Kullanılan Sınıflar](#paketler-ve-sık-kullanılan-sınıflar)

---

## Temel Kavramlar

### Değişkenler ve Veri Türleri

#### Primitif Türler
```java
// Tam sayılar
byte b = 127;           // 8-bit (-128 to 127)
short s = 32767;        // 16-bit (-32,768 to 32,767)
int i = 2147483647;     // 32-bit (-2^31 to 2^31-1)
long l = 9223372036854775807L; // 64-bit (-2^63 to 2^63-1)

// Ondalıklı sayılar
float f = 3.14f;        // 32-bit
double d = 3.14159265359; // 64-bit

// Diğer türler
char c = 'A';           // 16-bit Unicode karakter
boolean bool = true;    // true veya false
```

#### Referans Türleri
```java
String str = "Merhaba Dünya";
String str2 = new String("Java");
Object obj = new Object();
```

### Final Anahtar Kelimesi
```java
final int CONSTANT = 100;        // Değiştirilemez değişken
final String NAME = "Java";      // Sabit string

// Final sınıf (kalıtım yapılamaz)
final class FinalClass { }

// Final metot (override edilemez)
public final void finalMethod() { }
```

### Operatörler

#### Aritmetik Operatörler
```java
int a = 10, b = 3;
int toplam = a + b;      // 13
int fark = a - b;        // 7
int carpim = a * b;      // 30
int bolum = a / b;       // 3
int kalan = a % b;       // 1
```

#### Atama Operatörleri
```java
int x = 5;
x += 3;  // x = x + 3 (8)
x -= 2;  // x = x - 2 (6)
x *= 4;  // x = x * 4 (24)
x /= 3;  // x = x / 3 (8)
x %= 5;  // x = x % 5 (3)
```

#### Karşılaştırma Operatörleri
```java
int a = 10, b = 20;
boolean esit = (a == b);        // false
boolean esitDegil = (a != b);   // true
boolean kucuk = (a < b);        // true
boolean buyuk = (a > b);        // false
boolean kucukEsit = (a <= b);   // true
boolean buyukEsit = (a >= b);   // false
```

#### Mantıksal Operatörler
```java
boolean x = true, y = false;
boolean ve = x && y;        // false (VE)
boolean veya = x || y;      // true (VEYA)
boolean degil = !x;         // false (DEĞİL)
```

#### Bitsel Operatörler
```java
int a = 5;  // 0101
int b = 3;  // 0011
int bitwiseAnd = a & b;     // 0001 = 1
int bitwiseOr = a | b;      // 0111 = 7
int bitwiseXor = a ^ b;     // 0110 = 6
int bitwiseNot = ~a;        // 1010 = -6
int leftShift = a << 1;     // 1010 = 10
int rightShift = a >> 1;    // 0010 = 2
```

#### instanceof Operatörü
```java
String str = "Java";
boolean result = str instanceof String; // true
```

### Tür Dönüşümü

#### Genişletme (Implicit/Otomatik)
```java
int i = 100;
long l = i;         // int'den long'a otomatik dönüşüm
float f = i;        // int'den float'a otomatik dönüşüm
double d = f;       // float'tan double'a otomatik dönüşüm
```

#### Daraltma (Explicit/Manuel)
```java
double d = 3.14159;
int i = (int) d;        // 3 (ondalık kısmı kesilir)
float f = (float) d;    // 3.14159f

// String'den sayıya dönüşüm
String str = "123";
int num = Integer.parseInt(str);
double dbl = Double.parseDouble("3.14");
```

### Kapsam

#### Sınıf Kapsamı (Class Scope)
```java
public class Example {
    private int classVariable = 10; // Sınıf seviyesinde değişken
    
    public void method() {
        System.out.println(classVariable); // Erişilebilir
    }
}
```

#### Metot Kapsamı (Method Scope)
```java
public void exampleMethod() {
    int methodVariable = 20; // Sadece bu metot içinde geçerli
    // classVariable'a da erişilebilir
}
```

#### Blok Kapsamı (Block Scope)
```java
public void blockExample() {
    int x = 10;
    if (true) {
        int y = 20; // Sadece bu blok içinde geçerli
        System.out.println(x); // x'e erişilebilir
    }
    // System.out.println(y); // HATA: y burada erişilemez
}
```

---

## Kontrol Akışı

### Koşullu İfadeler

#### if-else-if-else
```java
int puan = 85;

if (puan >= 90) {
    System.out.println("A harf notu");
} else if (puan >= 80) {
    System.out.println("B harf notu");
} else if (puan >= 70) {
    System.out.println("C harf notu");
} else {
    System.out.println("Geçemedi");
}
```

#### switch
```java
// Geleneksel switch
int gun = 3;
switch (gun) {
    case 1:
        System.out.println("Pazartesi");
        break;
    case 2:
        System.out.println("Salı");
        break;
    case 3:
        System.out.println("Çarşamba");
        break;
    default:
        System.out.println("Bilinmeyen gün");
        break;
}

// Modern switch (Java 14+)
String gunAdi = switch (gun) {
    case 1 -> "Pazartesi";
    case 2 -> "Salı";
    case 3 -> "Çarşamba";
    default -> "Bilinmeyen gün";
};
```

#### Ternary Operator
```java
int a = 10, b = 20;
int max = (a > b) ? a : b; // b = 20
String durum = (puan >= 60) ? "Geçti" : "Kaldı";
```

### Döngüler

#### for Döngüsü
```java
// Geleneksel for döngüsü
for (int i = 0; i < 5; i++) {
    System.out.println("i = " + i);
}

// Çoklu değişkenli for
for (int i = 0, j = 10; i < 5; i++, j--) {
    System.out.println("i: " + i + ", j: " + j);
}
```

#### Enhanced for (for-each)
```java
int[] sayilar = {1, 2, 3, 4, 5};
for (int sayi : sayilar) {
    System.out.println(sayi);
}

// Koleksiyonlarla
List<String> isimler = Arrays.asList("Ali", "Veli", "Deli");
for (String isim : isimler) {
    System.out.println(isim);
}
```

#### while Döngüsü
```java
int i = 0;
while (i < 5) {
    System.out.println("i = " + i);
    i++;
}
```

#### do-while Döngüsü
```java
int i = 0;
do {
    System.out.println("i = " + i);
    i++;
} while (i < 5);
```

#### Döngü Kontrol İfadeleri
```java
for (int i = 0; i < 10; i++) {
    if (i == 3) {
        continue; // Bu iterasyonu atla
    }
    if (i == 7) {
        break; // Döngüden çık
    }
    System.out.println(i);
}
```

---

## Metotlar

### Metot Bildirimi ve Çağırma
```java
public class Calculator {
    // Metot bildirimi
    public int topla(int a, int b) {
        return a + b;
    }
    
    // void metot (geri dönüş değeri yok)
    public void mesajYaz(String mesaj) {
        System.out.println(mesaj);
    }
    
    // static metot
    public static double kareAl(double sayi) {
        return sayi * sayi;
    }
}

// Metot çağırma
Calculator calc = new Calculator();
int sonuc = calc.topla(5, 3);           // 8
calc.mesajYaz("Merhaba");               // void metot
double kare = Calculator.kareAl(4.0);   // static metot
```

### Parametreler ve Argümanlar

#### Değişken Sayıda Argüman (Varargs)
```java
public void yazdir(String... metinler) {
    for (String metin : metinler) {
        System.out.println(metin);
    }
}

// Kullanım
yazdir("Bir");
yazdir("Bir", "İki");
yazdir("Bir", "İki", "Üç");
```

#### Referans vs Değer Geçirme
```java
// Primitif türler - değer geçirme
public void degistir(int x) {
    x = 100; // Orijinal değişkeni etkilemez
}

// Nesneler - referans geçirme
public void listeEkle(List<String> liste) {
    liste.add("Yeni eleman"); // Orijinal listeyi etkiler
}
```

### Metot Aşırı Yüklemesi (Method Overloading)
```java
public class Calculator {
    public int topla(int a, int b) {
        return a + b;
    }
    
    public double topla(double a, double b) {
        return a + b;
    }
    
    public int topla(int a, int b, int c) {
        return a + b + c;
    }
    
    public String topla(String a, String b) {
        return a + b;
    }
}
```

### Lambda İfadeleri ve Fonksiyonel Arayüzler

#### Fonksiyonel Arayüz
```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}
```

#### Lambda İfadeleri
```java
// Geleneksel anonymous class
Calculator toplama = new Calculator() {
    @Override
    public int calculate(int a, int b) {
        return a + b;
    }
};

// Lambda ile
Calculator toplamaLambda = (a, b) -> a + b;
Calculator cikartmaLambda = (a, b) -> a - b;

// Kullanım
int sonuc1 = toplamaLambda.calculate(5, 3); // 8
int sonuc2 = cikartmaLambda.calculate(5, 3); // 2

// Yaygın fonksiyonel arayüzler
Predicate<String> bosDegilMi = s -> !s.isEmpty();
Function<String, Integer> uzunluk = s -> s.length();
Consumer<String> yazdir = s -> System.out.println(s);
Supplier<String> varsayilan = () -> "Varsayılan değer";
```

---

## Diziler ve Koleksiyonlar

### Diziler (Arrays)

#### Dizi Oluşturma
```java
// Farklı dizi oluşturma yöntemleri
int[] sayilar1 = new int[5];                    // [0,0,0,0,0]
int[] sayilar2 = {1, 2, 3, 4, 5};             // İlk değerlerle
int[] sayilar3 = new int[]{10, 20, 30};       // Alternatif sözdizimi

// Çok boyutlu diziler
int[][] matris = new int[3][4];                // 3x4 matris
int[][] matris2 = {{1,2}, {3,4}, {5,6}};     // İlk değerlerle
```

#### Temel Dizi İşlemleri
```java
int[] dizi = {10, 20, 30, 40, 50};

// Eleman erişimi
int ilkEleman = dizi[0];        // 10
int sonEleman = dizi[dizi.length - 1]; // 50

// Dizide dolaşma
for (int i = 0; i < dizi.length; i++) {
    System.out.println(dizi[i]);
}

// Enhanced for ile
for (int eleman : dizi) {
    System.out.println(eleman);
}

// Dizi kopyalama
int[] kopya = Arrays.copyOf(dizi, dizi.length);
int[] parcaKopya = Arrays.copyOfRange(dizi, 1, 4); // [20,30,40]

// Dizi sıralama
Arrays.sort(dizi);

// Arama
int index = Arrays.binarySearch(dizi, 30);
```

### Koleksiyonlar Çerçevesi (Collections Framework)

#### List Arayüzü

**ArrayList**
```java
// ArrayList oluşturma
List<String> liste = new ArrayList<>();
ArrayList<Integer> sayilar = new ArrayList<>();

// Eleman ekleme
liste.add("Java");
liste.add("Python");
liste.add(1, "C++"); // Belirli index'e ekleme

// Eleman erişimi
String ilk = liste.get(0);          // "Java"
int boyut = liste.size();           // 3

// Eleman güncelleme
liste.set(0, "JavaScript");         // İlk elemanı değiştir

// Eleman silme
liste.remove(1);                    // Index ile
liste.remove("Python");            // Değer ile

// Iterasyon
for (String dil : liste) {
    System.out.println(dil);
}
```

**LinkedList**
```java
LinkedList<String> bagli = new LinkedList<>();
bagli.addFirst("İlk");
bagli.addLast("Son");
bagli.add("Orta");

String ilk = bagli.removeFirst();
String son = bagli.removeLast();
```

#### Set Arayüzü

**HashSet**
```java
Set<String> hashSet = new HashSet<>();
hashSet.add("Java");
hashSet.add("Python");
hashSet.add("Java"); // Dublicate eklenmez

// Kontrol
boolean var = hashSet.contains("Java"); // true
int boyut = hashSet.size(); // 2

// Iterasyon
for (String eleman : hashSet) {
    System.out.println(eleman);
}
```

**TreeSet** (Sıralı Set)
```java
Set<Integer> treeSet = new TreeSet<>();
treeSet.add(30);
treeSet.add(10);
treeSet.add(20);
// Otomatik sıralanır: [10, 20, 30]
```

#### Map Arayüzü

**HashMap**
```java
Map<String, Integer> hashMap = new HashMap<>();

// Eleman ekleme
hashMap.put("Java", 25);
hashMap.put("Python", 30);
hashMap.put("C++", 35);

// Eleman erişimi
Integer yas = hashMap.get("Java");          // 25
Integer varsayilan = hashMap.getOrDefault("Go", 0); // 0

// Kontrol
boolean anahtarVar = hashMap.containsKey("Java");   // true
boolean degerVar = hashMap.containsValue(25);       // true

// Iterasyon
for (Map.Entry<String, Integer> entry : hashMap.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}

// Sadece anahtarlar
for (String anahtar : hashMap.keySet()) {
    System.out.println(anahtar);
}

// Sadece değerler
for (Integer deger : hashMap.values()) {
    System.out.println(deger);
}
```

**TreeMap** (Sıralı Map)
```java
Map<String, String> treeMap = new TreeMap<>();
treeMap.put("c", "C++");
treeMap.put("a", "Assembly");
treeMap.put("b", "BASIC");
// Anahtarlara göre sıralanır: a, b, c
```

### Stream API (Java 8+)

#### Stream Oluşturma
```java
List<Integer> sayilar = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
Stream<Integer> stream = sayilar.stream();

// Diziden stream
String[] dizgiler = {"a", "b", "c"};
Stream<String> dizgiStream = Arrays.stream(dizgiler);

// Stream.of ile
Stream<String> directStream = Stream.of("x", "y", "z");
```

#### Ara İşlemler (Intermediate Operations)

**filter()** - Filtreleme
```java
List<Integer> ciftSayilar = sayilar.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());
// [2, 4, 6, 8, 10]
```

**map()** - Dönüştürme
```java
List<Integer> kareler = sayilar.stream()
    .map(n -> n * n)
    .collect(Collectors.toList());
// [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

**sorted()** - Sıralama
```java
List<String> isimler = Arrays.asList("Zeynep", "Ali", "Mehmet");
List<String> siraliIsimler = isimler.stream()
    .sorted()
    .collect(Collectors.toList());
// [Ali, Mehmet, Zeynep]
```

**distinct()** - Tekrar eden elemanları kaldırma
```java
List<Integer> benzersizler = Arrays.asList(1, 2, 2, 3, 3, 4).stream()
    .distinct()
    .collect(Collectors.toList());
// [1, 2, 3, 4]
```

#### Son İşlemler (Terminal Operations)

**collect()** - Toplama
```java
List<String> liste = stream.collect(Collectors.toList());
Set<String> set = stream.collect(Collectors.toSet());
String birlestir = stream.collect(Collectors.joining(", "));
```

**forEach()** - Her eleman için işlem
```java
sayilar.stream()
    .filter(n -> n % 2 == 0)
    .forEach(System.out::println);
```

**reduce()** - İndirgeme
```java
Optional<Integer> toplam = sayilar.stream().reduce(Integer::sum);
int toplam2 = sayilar.stream().reduce(0, Integer::sum);
int carpim = sayilar.stream().reduce(1, (a, b) -> a * b);
```

**count()** - Sayma
```java
long ciftSayiAdedi = sayilar.stream()
    .filter(n -> n % 2 == 0)
    .count(); // 5
```

**anyMatch(), allMatch(), noneMatch()** - Kontrol
```java
boolean herhangiCift = sayilar.stream().anyMatch(n -> n % 2 == 0); // true
boolean hepsiPositif = sayilar.stream().allMatch(n -> n > 0); // true
boolean hicbirinegatif = sayilar.stream().noneMatch(n -> n < 0); // true
```

#### Kompleks Stream Örnekleri
```java
List<String> kelimeler = Arrays.asList("java", "python", "javascript", "c++", "go");

// En uzun kelimeyi bul
Optional<String> enUzun = kelimeler.stream()
    .max(Comparator.comparing(String::length));

// Uzunluğuna göre grupla
Map<Integer, List<String>> gruplama = kelimeler.stream()
    .collect(Collectors.groupingBy(String::length));

// Büyük harfe çevir ve birleştir
String sonuc = kelimeler.stream()
    .map(String::toUpperCase)
    .collect(Collectors.joining(" - "));
```

---

## Nesne Yönelimli Programlama

### Sınıflar ve Nesneler

#### Sınıf Tanımlama
```java
public class Araba {
    // Alan değişkenleri (fields)
    private String marka;
    private String model;
    private int yil;
    private static int arabaSayisi = 0; // static alan
    
    // Varsayılan constructor
    public Araba() {
        this("Bilinmeyen", "Bilinmeyen", 2000);
    }
    
    // Parametreli constructor
    public Araba(String marka, String model, int yil) {
        this.marka = marka;
        this.model = model;
        this.yil = yil;
        arabaSayisi++; // static alan artırılır
    }
    
    // Getter metotları
    public String getMarka() { return marka; }
    public String getModel() { return model; }
    public int getYil() { return yil; }
    
    // Setter metotları
    public void setMarka(String marka) { this.marka = marka; }
    public void setModel(String model) { this.model = model; }
    public void setYil(int yil) { this.yil = yil; }
    
    // static metot
    public static int getArabaSayisi() {
        return arabaSayisi;
    }
    
    // Instance metot
    public void bilgiYazdir() {
        System.out.println(yil + " " + marka + " " + model);
    }
    
    // toString metotu (Object sınıfından override)
    @Override
    public String toString() {
        return yil + " " + marka + " " + model;
    }
}
```

#### Nesne Oluşturma ve Kullanma
```java
// Nesne oluşturma
Araba araba1 = new Araba();
Araba araba2 = new Araba("Toyota", "Corolla", 2020);

// Metot çağırma
araba2.bilgiYazdir();
System.out.println(araba2.toString());

// Static metot çağırma
int toplam = Araba.getArabaSayisi();
```

### Erişim Belirteçleri

```java
public class ErisimOrnegi {
    public String herYerdenErisim;      // Her yerden erişilebilir
    protected String paketVeAltSinif;   // Aynı paket + alt sınıflar
    String paketErisimi;                // Sadece aynı paket (default)
    private String sadeceBuSinif;       // Sadece bu sınıf içinde
    
    public void publicMetot() { }       // Heryerden çağrılabilir
    protected void protectedMetot() { } // Paket + alt sınıflar
    void defaultMetot() { }             // Sadece aynı paket
    private void privateMetot() { }     // Sadece bu sınıf
}
```

### Kalıtım (Inheritance)

#### extends Anahtar Kelimesi
```java
// Ana sınıf (Parent/Super class)
public class Hayvan {
    protected String ad;
    protected int yas;
    
    public Hayvan(String ad, int yas) {
        this.ad = ad;
        this.yas = yas;
    }
    
    public void sesCikar() {
        System.out.println(ad + " ses çıkarıyor");
    }
    
    public void bilgiYazdir() {
        System.out.println("Ad: " + ad + ", Yaş: " + yas);
    }
}

// Alt sınıf (Child/Sub class)
public class Kopek extends Hayvan {
    private String cins;
    
    public Kopek(String ad, int yas, String cins) {
        super(ad, yas); // Ana sınıf constructor'unu çağır
        this.cins = cins;
    }
    
    // Metot override
    @Override
    public void sesCikar() {
        System.out.println(ad + " havlıyor: Hav hav!");
    }
    
    // Yeni metot
    public void oyunOyna() {
        System.out.println(ad + " top oynuyor");
    }
    
    @Override
    public void bilgiYazdir() {
        super.bilgiYazdir(); // Ana sınıfın metodunu çağır
        System.out.println("Cins: " + cins);
    }
}

// Kedi sınıfı
public class Kedi extends Hayvan {
    private String cins;
    
    public Kedi(String ad, int yas, String cins) {
        super(ad, yas);
        this.cins = cins;
    }
    
    @Override
    public void sesCikar() {
        System.out.println(ad + " miyavlıyor: Miyav!");
    }
    
    public void tirmalama() {
        System.out.println(ad + " tırmalıyor");
    }
}

// Kullanım
Kopek kopek = new Kopek("Karabaş", 3, "Alman Çobanı");
kopek.sesCikar();    // Override edilmiş metot
kopek.oyunOyna();    // Alt sınıfa özgü metot
kopek.bilgiYazdir(); // Override edilmiş metot

Kedi kedi = new Kedi("Pamuk", 1, "Tekir");
kedi.sesCikar();     // Override edilmiş metot
kedi.tirmalama();    // Alt sınıfa özgü metot
```

### Polimorfizm (Polymorphism)

```java
public class PolimorfizmOrnegi {
    public static void main(String[] args) {
        // Ana sınıf referansı, alt sınıf nesnesi
        Hayvan hayvan1 = new Kopek("Rex", 2, "Labrador");
        Hayvan hayvan2 = new Kedi("Pamuk", 1, "Tekir");
        
        // Runtime'da doğru metot çağrılır (dynamic binding)
        hayvan1.sesCikar(); // "Rex havlıyor: Hav hav!"
        hayvan2.sesCikar(); // "Pamuk miyavlıyor: Miyav!"
        
        // Polimorfik dizi
        Hayvan[] hayvanlar = {
            new Kopek("Buddy", 3, "Bulldog"),
            new Kedi("Whiskers", 2, "Siyam")
        };
        
        for (Hayvan h : hayvanlar) {
            h.sesCikar(); // Her biri kendi sesini çıkarır
        }
        
        // instanceof ile tür kontrolü
        if (hayvan1 instanceof Kopek) {
            Kopek k = (Kopek) hayvan1; // Type casting
            k.oyunOyna();
        }
    }
}
```

### Soyut Sınıflar (Abstract Classes)

```java
// Soyut sınıf
public abstract class Sekil {
    protected String renk;
    
    public Sekil(String renk) {
        this.renk = renk;
    }
    
    // Soyut metot - alt sınıflarda implement edilmeli
    public abstract double alanHesapla();
    
    // Concrete metot - alt sınıflarda kullanılabilir
    public void renkYazdir() {
        System.out.println("Renk: " + renk);
    }
}

// Soyut sınıftan türetme
public class Dikdortgen extends Sekil {
    private double genislik, yukseklik;
    
    public Dikdortgen(String renk, double genislik, double yukseklik) {
        super(renk);
        this.genislik = genislik;
        this.yukseklik = yukseklik;
    }
    
    @Override
    public double alanHesapla() {
        return genislik * yukseklik;
    }
}

public class Daire extends Sekil {
    private double yaricap;
    
    public Daire(String renk, double yaricap) {
        super(renk);
        this.yaricap = yaricap;
    }
    
    @Override
    public double alanHesapla() {
        return Math.PI * yaricap * yaricap;
    }
}
```

### Arayüzler (Interfaces)

```java
// Arayüz tanımlama
public interface Ucabilir {
    // Sabitler (otomatik public static final)
    int MAKSIMUM_YUKSEKLIK = 10000;
    
    // Soyut metotlar (otomatik public abstract)
    void ucmayaBashla();
    void inis();
    
    // Default metot (Java 8+)
    default void ucusBilgisi() {
        System.out.println("Uçuş başlıyor...");
    }
    
    // Static metot (Java 8+)
    static void guvenlikKontrolu() {
        System.out.println("Güvenlik kontrolü yapılıyor");
    }
}

// Arayüz implementation
public class Ucak implements Ucabilir {
    private String model;
    
    public Ucak(String model) {
        this.model = model;
    }
    
    @Override
    public void ucmayaBashla() {
        System.out.println(model + " uçmaya başlıyor");
    }
    
    @Override
    public void inis() {
        System.out.println(model + " iniş yapıyor");
    }
}

// Çoklu arayüz implementation
public interface Yuzebilir {
    void yuzmeyeBasla();
}

public class Amfibi implements Ucabilir, Yuzebilir {
    @Override
    public void ucmayaBashla() { /* ... */ }
    
    @Override
    public void inis() { /* ... */ }
    
    @Override
    public void yuzmeyeBasla() { /* ... */ }
}
```

---

## Hata Yönetimi

### try-catch-finally

#### Temel Kullanım
```java
public class HataYonetimi {
    public static void bolmeIslemi(int a, int b) {
        try {
            int sonuc = a / b;
            System.out.println("Sonuç: " + sonuc);
        } catch (ArithmeticException e) {
            System.out.println("Sıfıra bölme hatası: " + e.getMessage());
        } finally {
            System.out.println("Bu blok her zaman çalışır");
        }
    }
}
```

#### Çoklu catch Blokları
```java
public void dosyaOku(String dosyaAdi) {
    try {
        FileReader file = new FileReader(dosyaAdi);
        BufferedReader reader = new BufferedReader(file);
        String satir = reader.readLine();
        int sayi = Integer.parseInt(satir);
        reader.close();
    } catch (FileNotFoundException e) {
        System.out.println("Dosya bulunamadı: " + e.getMessage());
    } catch (IOException e) {
        System.out.println("Dosya okuma hatası: " + e.getMessage());
    } catch (NumberFormatException e) {
        System.out.println("Sayı formatı hatası: " + e.getMessage());
    } catch (Exception e) {
        System.out.println("Genel hata: " + e.getMessage());
    }
}
```

#### Multi-catch (Java 7+)
```java
try {
    // Risky code
} catch (IOException | SQLException e) {
    System.out.println("IO veya SQL hatası: " + e.getMessage());
}
```

#### try-with-resources (Java 7+)
```java
// Otomatik resource yönetimi
public void dosyaOku(String dosyaAdi) {
    try (FileReader file = new FileReader(dosyaAdi);
         BufferedReader reader = new BufferedReader(file)) {
        
        String satir = reader.readLine();
        System.out.println(satir);
        
    } catch (IOException e) {
        System.out.println("Dosya hatası: " + e.getMessage());
    }
    // reader ve file otomatik olarak kapatılır
}
```

### throw ve throws

#### throw - Hata Fırlatma
```java
public void yasKontrol(int yas) {
    if (yas < 0) {
        throw new IllegalArgumentException("Yaş negatif olamaz: " + yas);
    }
    if (yas > 150) {
        throw new IllegalArgumentException("Geçersiz yaş: " + yas);
    }
    System.out.println("Geçerli yaş: " + yas);
}
```

#### throws - Hata Bildirimi
```java
// Checked exception'ı declare etme
public void dosyaYaz(String dosyaAdi) throws IOException {
    FileWriter writer = new FileWriter(dosyaAdi);
    writer.write("Merhaba Dünya");
    writer.close();
}

// Metodu çağıran try-catch kullanmalı
public void dosyaIslemleri() {
    try {
        dosyaYaz("test.txt");
    } catch (IOException e) {
        System.out.println("Dosya yazma hatası: " + e.getMessage());
    }
}
```

### İstisna Türleri

#### Checked vs Unchecked Exceptions
```java
// Unchecked Exception (Runtime Exception)
public class UncheckedOrnegi {
    public void arrayHatasi() {
        int[] dizi = {1, 2, 3};
        System.out.println(dizi[5]); // ArrayIndexOutOfBoundsException
    }
    
    public void nullPointerHatasi() {
        String str = null;
        System.out.println(str.length()); // NullPointerException
    }
}

// Checked Exception (Compile-time Exception)
public class CheckedOrnegi {
    public void dosyaOku() throws FileNotFoundException, IOException {
        FileReader file = new FileReader("dosya.txt"); // FileNotFoundException
        file.read(); // IOException
    }
}
```

#### Custom Exception Oluşturma
```java
// Custom checked exception
public class YetersizBakiyeException extends Exception {
    private double eksikMiktar;
    
    public YetersizBakiyeException(String message, double eksikMiktar) {
        super(message);
        this.eksikMiktar = eksikMiktar;
    }
    
    public double getEksikMiktar() {
        return eksikMiktar;
    }
}

// Custom unchecked exception
public class GecersizHesapException extends RuntimeException {
    public GecersizHesapException(String message) {
        super(message);
    }
}

// Kullanım
public class BankaHesabi {
    private double bakiye;
    
    public void paraÇek(double miktar) throws YetersizBakiyeException {
        if (miktar > bakiye) {
            throw new YetersizBakiyeException(
                "Yetersiz bakiye", miktar - bakiye);
        }
        bakiye -= miktar;
    }
}
```

---

## Eşzamanlılık

### Thread Sınıfı ve Runnable Arayüzü

#### Thread Sınıfından Türetme
```java
public class MyThread extends Thread {
    private String isim;
    
    public MyThread(String isim) {
        this.isim = isim;
    }
    
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(isim + " - Adım: " + i);
            try {
                Thread.sleep(1000); // 1 saniye bekle
            } catch (InterruptedException e) {
                System.out.println(isim + " kesintiye uğradı");
                return;
            }
        }
        System.out.println(isim + " tamamlandı");
    }
}

// Kullanım
MyThread thread1 = new MyThread("Thread-1");
MyThread thread2 = new MyThread("Thread-2");

thread1.start(); // run() metodunu çağırır
thread2.start();
```

#### Runnable Arayüzü Implementation
```java
public class MyRunnable implements Runnable {
    private String isim;
    
    public MyRunnable(String isim) {
        this.isim = isim;
    }
    
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(isim + " - Adım: " + i);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                System.out.println(isim + " kesintiye uğradı");
                return;
            }
        }
    }
}

// Kullanım
MyRunnable runnable1 = new MyRunnable("Runnable-1");
MyRunnable runnable2 = new MyRunnable("Runnable-2");

Thread thread1 = new Thread(runnable1);
Thread thread2 = new Thread(runnable2);

thread1.start();
thread2.start();
```

#### Lambda ile Thread
```java
// Lambda ile Runnable
Thread lambdaThread = new Thread(() -> {
    for (int i = 1; i <= 3; i++) {
        System.out.println("Lambda Thread - " + i);
        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
});

lambdaThread.start();
```

#### Thread Kontrol Metotları
```java
public class ThreadKontrol {
    public static void main(String[] args) {
        Thread worker = new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                System.out.println("Çalışıyor: " + i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    System.out.println("Thread kesintiye uğradı");
                    return;
                }
            }
        });
        
        worker.start();
        
        try {
            Thread.sleep(3000);
            worker.interrupt(); // Thread'i kesintiye uğrat
            worker.join(); // Thread'in bitmesini bekle
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        System.out.println("Ana thread bitti");
    }
}
```

### Senkronizasyon

#### synchronized Anahtar Kelimesi
```java
public class SenkronizeOrnek {
    private int sayac = 0;
    
    // Synchronized metot
    public synchronized void arttir() {
        sayac++;
        System.out.println("Sayaç: " + sayac + " - Thread: " + 
                         Thread.currentThread().getName());
    }
    
    // Synchronized blok
    public void azalt() {
        synchronized (this) {
            sayac--;
            System.out.println("Sayaç: " + sayac + " - Thread: " + 
                             Thread.currentThread().getName());
        }
    }
    
    public int getSayac() {
        return sayac;
    }
}

// Kullanım
SenkronizeOrnek obj = new SenkronizeOrnek();

// Birden fazla thread aynı nesne üzerinde çalışır
for (int i = 0; i < 3; i++) {
    new Thread(() -> {
        for (int j = 0; j < 5; j++) {
            obj.arttir();
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }).start();
}
```

### CompletableFuture ile Asenkron Programlama

#### Temel CompletableFuture Kullanımı
```java
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutionException;

public class CompletableFutureOrnek {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // Basit asenkron görev
        CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(2000);
                return "Merhaba";
            } catch (InterruptedException e) {
                return "Hata";
            }
        });
        
        // Sonucu al (blocking)
        String sonuc1 = future1.get();
        System.out.println(sonuc1);
        
        // Non-blocking callback
        CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> {
            return "Dünya";
        }).thenApply(s -> s.toUpperCase())
          .thenApply(s -> "Merhaba " + s + "!");
        
        future2.thenAccept(System.out::println);
        
        // Birden fazla future'ı birleştirme
        CompletableFuture<String> future3 = CompletableFuture.supplyAsync(() -> "Java");
        CompletableFuture<String> future4 = CompletableFuture.supplyAsync(() -> "Programming");
        
        CompletableFuture<String> birlestirme = future3.thenCombine(future4, 
            (s1, s2) -> s1 + " " + s2);
        
        System.out.println(birlestirme.get());
    }
}
```

#### Zincir İşlemler ve Hata Yönetimi
```java
CompletableFuture<String> kompleksIslem = CompletableFuture
    .supplyAsync(() -> {
        // İlk görev
        System.out.println("1. İşlem başladı");
        return "İlk";
    })
    .thenApply(s -> {
        // İkinci görev
        System.out.println("2. İşlem: " + s);
        return s + " İkinci";
    })
    .thenCompose(s -> {
        // Başka bir CompletableFuture döndür
        return CompletableFuture.supplyAsync(() -> s + " Üçüncü");
    })
    .handle((sonuc, hata) -> {
        // Hata yönetimi
        if (hata != null) {
            return "Hata oluştu: " + hata.getMessage();
        }
        return sonuc + " Tamamlandı";
    });

kompleksIslem.thenAccept(System.out::println);
```

#### Paralel İşleme
```java
List<CompletableFuture<String>> futures = Arrays.asList("A", "B", "C", "D")
    .stream()
    .map(s -> CompletableFuture.supplyAsync(() -> {
        try {
            Thread.sleep(1000);
            return "İşlendi: " + s;
        } catch (InterruptedException e) {
            return "Hata: " + s;
        }
    }))
    .collect(Collectors.toList());

// Tüm future'ların bitmesini bekle
CompletableFuture<Void> tumIslemler = CompletableFuture.allOf(
    futures.toArray(new CompletableFuture[0]));

tumIslemler.thenRun(() -> {
    futures.forEach(f -> {
        try {
            System.out.println(f.get());
        } catch (Exception e) {
            e.printStackTrace();
        }
    });
});
```

---

## Modern Java Özellikleri

### Lambda İfadeleri ve Stream API (Detaylı)

#### Method References
```java
List<String> isimler = Arrays.asList("Ali", "Veli", "Ayşe", "Fatma");

// Lambda ifadesi
isimler.forEach(isim -> System.out.println(isim));

// Method reference
isimler.forEach(System.out::println);

// Static method reference
List<Integer> sayilar = Arrays.asList("1", "2", "3").stream()
    .map(Integer::parseInt)
    .collect(Collectors.toList());

// Instance method reference
List<String> buyukHarfler = isimler.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());

// Constructor reference
List<Integer> uzunluklar = isimler.stream()
    .map(String::length)
    .map(Integer::new)
    .collect(Collectors.toList());
```

#### Karmaşık Stream İşlemleri
```java
public class Ogrenci {
    private String ad;
    private int yas;
    private double not;
    private String bolum;
    
    // Constructor, getters, setters...
}

List<Ogrenci> ogrenciler = Arrays.asList(
    new Ogrenci("Ali", 20, 85.5, "Bilgisayar"),
    new Ogrenci("Ayşe", 19, 92.0, "Matematik"),
    new Ogrenci("Mehmet", 21, 78.0, "Bilgisayar"),
    new Ogrenci("Zeynep", 20, 88.5, "Matematik")
);

// Bölüme göre gruplama
Map<String, List<Ogrenci>> bolumGrubu = ogrenciler.stream()
    .collect(Collectors.groupingBy(Ogrenci::getBolum));

// Yaşa göre gruplama ve ortalama hesaplama
Map<Integer, Double> yasOrtalama = ogrenciler.stream()
    .collect(Collectors.groupingBy(
        Ogrenci::getYas,
        Collectors.averagingDouble(Ogrenci::getNot)
    ));

// En yüksek notu alan öğrenci
Optional<Ogrenci> enBasarili = ogrenciler.stream()
    .max(Comparator.comparing(Ogrenci::getNot));

// Bölüm bazında en başarılı öğrenciler
Map<String, Optional<Ogrenci>> bolumEnBasarili = ogrenciler.stream()
    .collect(Collectors.groupingBy(
        Ogrenci::getBolum,
        Collectors.maxBy(Comparator.comparing(Ogrenci::getNot))
    ));

// 80+ notu olan öğrenci sayısı
long basariliSayisi = ogrenciler.stream()
    .filter(o -> o.getNot() >= 80)
    .count();

// Yaş sıralaması ve ilk 2 öğrenci
List<Ogrenci> enGencler = ogrenciler.stream()
    .sorted(Comparator.comparing(Ogrenci::getYas))
    .limit(2)
    .collect(Collectors.toList());
```

### Optional Sınıfı

#### Optional Oluşturma
```java
// Boş Optional
Optional<String> bos = Optional.empty();

// Değer ile Optional
Optional<String> deger = Optional.of("Java");

// Null olabilecek değer ile Optional
String nullDeger = null;
Optional<String> nullableOptional = Optional.ofNullable(nullDeger);
```

#### Optional Kullanımı
```java
public class OptionalOrnegi {
    public Optional<String> kullaniciAdiBul(int id) {
        if (id == 1) {
            return Optional.of("Admin");
        }
        return Optional.empty();
    }
    
    public void ornekKullanim() {
        Optional<String> kullanici = kullaniciAdiBul(1);
        
        // Klasik kontrol
        if (kullanici.isPresent()) {
            System.out.println("Kullanıcı: " + kullanici.get());
        }
        
        // Modern yaklaşım
        kullanici.ifPresent(ad -> System.out.println("Kullanıcı: " + ad));
        
        // Varsayılan değer
        String ad = kullanici.orElse("Bilinmeyen");
        String ad2 = kullanici.orElseGet(() -> "Varsayılan Kullanıcı");
        
        // Exception fırlatma
        try {
            String ad3 = kullanici.orElseThrow(() -> 
                new RuntimeException("Kullanıcı bulunamadı"));
        } catch (RuntimeException e) {
            System.out.println(e.getMessage());
        }
        
        // Dönüştürme
        Optional<Integer> uzunluk = kullanici.map(String::length);
        Optional<String> buyukHarf = kullanici.map(String::toUpperCase);
        
        // Filtreleme
        Optional<String> uzunAdlar = kullanici.filter(ad -> ad.length() > 3);
        
        // FlatMap
        Optional<String> emailOptional = kullanici
            .flatMap(this::emailBul);
    }
    
    private Optional<String> emailBul(String kullaniciAdi) {
        return Optional.of(kullaniciAdi + "@example.com");
    }
}
```

### Yeni Tarih ve Zaman API'si (java.time)

#### Temel Sınıflar
```java
import java.time.*;
import java.time.format.DateTimeFormatter;

public class TarihZamanOrnegi {
    public void temelKullanim() {
        // Mevcut tarih ve zaman
        LocalDate bugun = LocalDate.now();
        LocalTime simdi = LocalTime.now();
        LocalDateTime simdiBurada = LocalDateTime.now();
        
        // Belirli tarih ve zaman oluşturma
        LocalDate dogumGunu = LocalDate.of(1990, 5, 15);
        LocalTime ogleSaati = LocalTime.of(12, 30, 0);
        LocalDateTime randevu = LocalDateTime.of(2024, 3, 15, 14, 30);
        
        // Zaman dilimi ile
        ZonedDateTime istanbulZamani = ZonedDateTime.now(ZoneId.of("Europe/Istanbul"));
        ZonedDateTime newYorkZamani = ZonedDateTime.now(ZoneId.of("America/New_York"));
        
        // Formatla
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd/MM/yyyy HH:mm");
        String formatlı = simdiBurada.format(formatter);
        
        // String'den parse etme
        LocalDate parsedDate = LocalDate.parse("2024-03-15");
        LocalDateTime parsedDateTime = LocalDateTime.parse("15/03/2024 14:30", formatter);
        
        System.out.println("Bugün: " + bugun);
        System.out.println("Şimdi: " + simdi);
        System.out.println("Formatlanmış: " + formatlı);
    }
}
```

#### Tarih Hesaplamaları
```java
LocalDate bugun = LocalDate.now();

// Tarih ekleme/çıkarma
LocalDate gelecekHafta = bugun.plusWeeks(1);
LocalDate gecenAy = bugun.minusMonths(1);
LocalDate ikiYilSonra = bugun.plusYears(2);

// Belirli gün ayarlama
LocalDate ayinIlki = bugun.withDayOfMonth(1);
LocalDate yilinIlki = bugun.withDayOfYear(1);

// Tarih karşılaştırma
LocalDate hedefTarih = LocalDate.of(2024, 12, 25);
boolean gecmiste = hedefTarih.isBefore(bugun);
boolean gelecekte = hedefTarih.isAfter(bugun);

// Period ve Duration
Period fark = Period.between(bugun, hedefTarih);
System.out.println(fark.getYears() + " yıl, " + 
                  fark.getMonths() + " ay, " + 
                  fark.getDays() + " gün");

// Zaman farkı
LocalTime baslangic = LocalTime.of(9, 0);
LocalTime bitis = LocalTime.of(17, 30);
Duration calismaSuresi = Duration.between(baslangic, bitis);
System.out.println("Çalışma süresi: " + calismaSuresi.toHours() + " saat");
```

### Metin Blokları (Text Blocks) - Java 15+

```java
public class MetinBloklari {
    public void ornekler() {
        // Geleneksel string
        String html1 = "<html>\n" +
                      "    <body>\n" +
                      "        <p>Merhaba Dünya</p>\n" +
                      "    </body>\n" +
                      "</html>";
        
        // Text block ile
        String html2 = """
                      <html>
                          <body>
                              <p>Merhaba Dünya</p>
                          </body>
                      </html>
                      """;
        
        // JSON örneği
        String json = """
                     {
                         "name": "John Doe",
                         "age": 30,
                         "city": "Istanbul"
                     }
                     """;
        
        // SQL örneği
        String sql = """
                    SELECT u.name, u.email, p.title
                    FROM users u
                    INNER JOIN posts p ON u.id = p.user_id
                    WHERE u.active = true
                    ORDER BY u.name
                    """;
        
        System.out.println(html2);
        System.out.println(json);
    }
}
```

### var Anahtar Kelimesi (Java 10+)

```java
public class VarOrnekleri {
    public void typeInference() {
        // Type inference ile değişken tanımlama
        var sayi = 42;                    // int çıkarımı
        var metin = "Merhaba Java";        // String çıkarımı
        var liste = new ArrayList<String>(); // ArrayList<String> çıkarımı
        var map = new HashMap<String, Integer>(); // HashMap<String, Integer> çıkarımı
        
        // Loop'larda kullanım
        var sayilar = Arrays.asList(1, 2, 3, 4, 5);
        for (var eleman : sayilar) {
            System.out.println(eleman);
        }
        
        // Stream'lerde kullanım
        var sonuc = sayilar.stream()
            .filter(x -> x > 2)
            .collect(Collectors.toList());
        
        // KISITLAMALAR:
        // var x; // HATA: İlk değer verilmeli
        // var y = null; // HATA: null değer verilemez
        // var z = {1, 2, 3}; // HATA: array initializer kullanılamaz
    }
}
```

### Records (Java 14+)

```java
// Geleneksel POJO
public class GelenekselKisi {
    private final String ad;
    private final int yas;
    
    public GelenekselKisi(String ad, int yas) {
        this.ad = ad;
        this.yas = yas;
    }
    
    public String getAd() { return ad; }
    public int getYas() { return yas; }
    
    @Override
    public boolean equals(Object obj) {
        // equals implementasyonu...
    }
    
    @Override
    public int hashCode() {
        // hashCode implementasyonu...
    }
    
    @Override
    public String toString() {
        return "Kisi[ad=" + ad + ", yas=" + yas + "]";
    }
}

// Record ile aynı fonksiyonalite
public record Kisi(String ad, int yas) {
    // Otomatik olarak constructor, getters, equals, hashCode, toString oluşturulur
    
    // Compact constructor (validation için)
    public Kisi {
        if (yas < 0) {
            throw new IllegalArgumentException("Yaş negatif olamaz");
        }
    }
    
    // Ek metotlar eklenebilir
    public boolean yetiskinMi() {
        return yas >= 18;
    }
    
    // Static factory metot
    public static Kisi cocuk(String ad) {
        return new Kisi(ad, 0);
    }
}

// Kullanım
Kisi kisi1 = new Kisi("Ali", 25);
Kisi kisi2 = new Kisi("Ayşe", 30);

// Getter metotları (ad(), yas())
System.out.println(kisi1.ad() + " - " + kisi1.yas());

// Otomatik equals/hashCode
boolean esitMi = kisi1.equals(kisi2); // false

// toString
System.out.println(kisi1); // Kisi[ad=Ali, yas=25]

// Pattern matching ile (Java 19+)
if (kisi1 instanceof Kisi(var ad, var yas)) {
    System.out.println("Ad: " + ad + ", Yaş: " + yas);
}
```

### Sealed Classes (Java 17+)

```java
// Sealed class - sadece belirtilen sınıflar extend edebilir
public abstract sealed class Sekil 
    permits Daire, Dikdortgen, Ucgen {
    
    public abstract double alanHesapla();
}

// Final sınıf (daha fazla extend edilemez)
public final class Daire extends Sekil {
    private final double yaricap;
    
    public Daire(double yaricap) {
        this.yaricap = yaricap;
    }
    
    @Override
    public double alanHesapla() {
        return Math.PI * yaricap * yaricap;
    }
}

// Non-sealed sınıf (başka sınıflar extend edebilir)
public non-sealed class Dikdortgen extends Sekil {
    private final double genislik, yukseklik;
    
    public Dikdortgen(double genislik, double yukseklik) {
        this.genislik = genislik;
        this.yukseklik = yukseklik;
    }
    
    @Override
    public double alanHesapla() {
        return genislik * yukseklik;
    }
}

// Sealed sınıf (sadece kendi alt paketindeki sınıflar extend edebilir)
public sealed class Ucgen extends Sekil {
    private final double taban, yukseklik;
    
    public Ucgen(double taban, double yukseklik) {
        this.taban = taban;
        this.yukseklik = yukseklik;
    }
    
    @Override
    public double alanHesapla() {
        return 0.5 * taban * yukseklik;
    }
}

// Pattern matching ile switch (Java 19+)
public String sekilBilgisi(Sekil sekil) {
    return switch (sekil) {
        case Daire d -> "Daire - Yarıçap: " + d.yaricap;
        case Dikdortgen d -> "Dikdörtgen - " + d.genislik + "x" + d.yukseklik;
        case Ucgen u -> "Üçgen - Taban: " + u.taban;
        // default gerekli değil - tüm alt sınıflar kapsanmış
    };
}
```

### Switch Expressions (Java 14+)

```java
public class SwitchExpressions {
    // Geleneksel switch
    public String gelenekselSwitch(int gun) {
        String gunAdi;
        switch (gun) {
            case 1:
                gunAdi = "Pazartesi";
                break;
            case 2:
                gunAdi = "Salı";
                break;
            case 3:
                gunAdi = "Çarşamba";
                break;
            default:
                gunAdi = "Bilinmeyen";
                break;
        }
        return gunAdi;
    }
    
    // Modern switch expression
    public String modernSwitch(int gun) {
        return switch (gun) {
            case 1 -> "Pazartesi";
            case 2 -> "Salı";
            case 3 -> "Çarşamba";
            case 4 -> "Perşembe";
            case 5 -> "Cuma";
            case 6, 7 -> "Hafta sonu";
            default -> "Bilinmeyen gün";
        };
    }
    
    // Blok ile switch expression
    public String karmasikSwitch(String mevsim) {
        return switch (mevsim.toLowerCase()) {
            case "kis" -> {
                System.out.println("Soğuk mevsim");
                yield "Kış ayları";
            }
            case "yaz" -> {
                System.out.println("Sıcak mevsim");
                yield "Yaz ayları";
            }
            case "ilkbahar", "sonbahar" -> "Geçiş mevsimleri";
            default -> throw new IllegalArgumentException("Bilinmeyen mevsim: " + mevsim);
        };
    }
}
```

---

## Generics

### Generic Sınıflar

#### Basit Generic Sınıf
```java
public class Kutu<T> {
    private T icerik;
    
    public void koy(T icerik) {
        this.icerik = icerik;
    }
    
    public T al() {
        return icerik;
    }
    
    public boolean bosmu() {
        return icerik == null;
    }
}

// Kullanım
Kutu<String> stringKutu = new Kutu<>();
stringKutu.koy("Merhaba");
String mesaj = stringKutu.al(); // Type-safe, casting gerekmez

Kutu<Integer> intKutu = new Kutu<>();
intKutu.koy(42);
Integer sayi = intKutu.al();
```

#### Çoklu Generic Parametreler
```java
public class Cift<K, V> {
    private K anahtar;
    private V deger;
    
    public Cift(K anahtar, V deger) {
        this.anahtar = anahtar;
        this.deger = deger;
    }
    
    public K getAnahtar() { return anahtar; }
    public V getDeger() { return deger; }
    
    public void setDeger(V deger) { this.deger = deger; }
}

// Kullanım
Cift<String, Integer> yaslar = new Cift<>("Ali", 25);
Cift<Integer, String> isimler = new Cift<>(1, "Java");
```

### Generic Metotlar

```java
public class GenericMetotlar {
    // Generic method
    public static <T> void swap(T[] dizi, int i, int j) {
        T temp = dizi[i];
        dizi[i] = dizi[j];
        dizi[j] = temp;
    }
    
    // Generic method with return type
    public static <T> T orta(T[] dizi) {
        return dizi[dizi.length / 2];
    }
    
    // Bounded type parameter
    public static <T extends Number> double toplam(T[] sayilar) {
        double toplam = 0.0;
        for (T sayi : sayilar) {
            toplam += sayi.doubleValue();
        }
        return toplam;
    }
}

// Kullanım
String[] isimler = {"Ali", "Veli", "Ayşe"};
GenericMetotlar.swap(isimler, 0, 2); // ["Ayşe", "Veli", "Ali"]

Integer[] sayilar = {1, 2, 3, 4, 5};
Integer ortaSayi = GenericMetotlar.orta(sayilar); // 3

Double[] ondaliklar = {1.5, 2.5, 3.5};
double toplam = GenericMetotlar.toplam(ondaliklar); // 7.5
```

### Wildcards

#### Unbounded Wildcards (?)
```java
public static void yazdirListe(List<?> liste) {
    for (Object eleman : liste) {
        System.out.println(eleman);
    }
}

// Herhangi bir List türü kabul edilir
yazdirListe(Arrays.asList(1, 2, 3));
yazdirListe(Arrays.asList("a", "b", "c"));
```

#### Upper Bounded Wildcards (? extends)
```java
public static double sayilarinToplami(List<? extends Number> liste) {
    double toplam = 0.0;
    for (Number sayi : liste) {
        toplam += sayi.doubleValue();
    }
    return toplam;
}

// Number ve alt sınıfları kabul edilir
List<Integer> integerListe = Arrays.asList(1, 2, 3);
List<Double> doubleListe = Arrays.asList(1.5, 2.5, 3.5);
double toplam1 = sayilarinToplami(integerListe); // 6.0
double toplam2 = sayilarinToplami(doubleListe);  // 7.5
```

#### Lower Bounded Wildcards (? super)
```java
public static void sayiEkle(List<? super Integer> liste) {
    liste.add(42);
    liste.add(100);
}

// Integer ve üst sınıfları kabul edilir
List<Number> numberListe = new ArrayList<>();
List<Object> objectListe = new ArrayList<>();
sayiEkle(numberListe); // OK
sayiEkle(objectListe); // OK
```

### Generic Inheritance
```java
// Generic interface
interface Karsilastirabilir<T> {
    int karsilastir(T other);
}

// Generic abstract class
abstract class Hayvan<T extends Hayvan<T>> implements Karsilastirabilir<T> {
    protected String ad;
    protected int yas;
    
    public Hayvan(String ad, int yas) {
        this.ad = ad;
        this.yas = yas;
    }
    
    @Override
    public int karsilastir(T other) {
        return Integer.compare(this.yas, other.yas);
    }
}

// Concrete implementation
class Kopek extends Hayvan<Kopek> {
    public Kopek(String ad, int yas) {
        super(ad, yas);
    }
}

// Kullanım
Kopek kopek1 = new Kopek("Rex", 3);
Kopek kopek2 = new Kopek("Max", 5);
int sonuc = kopek1.karsilastir(kopek2); // -1 (Rex daha genç)
```

### Type Erasure ve Raw Types
```java
// Raw type (önerilmez)
List rawList = new ArrayList(); // Uyarı verir
rawList.add("string");
rawList.add(42);

// Generic type
List<String> stringList = new ArrayList<>();
// stringList.add(42); // Compile error

// Type erasure sonrası runtime'da tüm generic bilgisi kaybolur
List<String> strings = new ArrayList<>();
List<Integer> integers = new ArrayList<>();
// strings.getClass() == integers.getClass() // true
```

---

## Annotations

### Built-in Annotations

#### Yaygın Annotations
```java
public class AnnotationOrnekleri {
    
    @Override
    public String toString() {
        return "Örnek sınıf";
    }
    
    @Deprecated
    public void eskiMetot() {
        System.out.println("Bu metot artık kullanılmamalı");
    }
    
    @SuppressWarnings("unchecked")
    public void uyariSizMetot() {
        List rawList = new ArrayList(); // Uyarı gösterilmez
        rawList.add("test");
    }
    
    @SafeVarargs
    public static <T> void varArgsMetot(T... elements) {
        for (T element : elements) {
            System.out.println(element);
        }
    }
}

// Kullanım
AnnotationOrnekleri ornek = new AnnotationOrnekleri();
ornek.eskiMetot(); // Deprecated uyarısı
AnnotationOrnekleri.varArgsMetot("a", "b", "c");
```

### Custom Annotations

#### Basit Custom Annotation
```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Test {
    String value() default "";
    int timeout() default 1000;
}

// Kullanım
public class TestSinifi {
    @Test
    public void basitTest() {
        System.out.println("Test çalışıyor");
    }
    
    @Test(value = "Önemli test", timeout = 5000)
    public void onemliTest() {
        System.out.println("Önemli test çalışıyor");
    }
}
```

#### Kompleks Custom Annotation
```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.METHOD})
@Documented
@Inherited
public @interface APIEndpoint {
    String path();
    HTTPMethod method() default HTTPMethod.GET;
    String[] permissions() default {};
    boolean auth() default true;
}

enum HTTPMethod {
    GET, POST, PUT, DELETE, PATCH
}

// Kullanım
@APIEndpoint(path = "/users", method = HTTPMethod.GET)
public class UserController {
    
    @APIEndpoint(
        path = "/users/{id}", 
        method = HTTPMethod.DELETE,
        permissions = {"ADMIN", "USER_DELETE"},
        auth = true
    )
    public void deleteUser(int id) {
        // User silme logic
    }
}
```

### Meta-Annotations

```java
// Retention Policy
@Retention(RetentionPolicy.SOURCE)   // Sadece source code'da
@Retention(RetentionPolicy.CLASS)    // Compile time'da (default)
@Retention(RetentionPolicy.RUNTIME)  // Runtime'da da mevcut

// Target
@Target(ElementType.TYPE)           // Class, interface, enum
@Target(ElementType.METHOD)         // Metotlar
@Target(ElementType.FIELD)          // Fields
@Target(ElementType.PARAMETER)      // Method parametreleri
@Target(ElementType.CONSTRUCTOR)    // Constructors
@Target(ElementType.LOCAL_VARIABLE) // Local variables
@Target(ElementType.ANNOTATION_TYPE) // Annotation types
@Target(ElementType.PACKAGE)        // Package

// Diğerleri
@Documented    // JavaDoc'a dahil edilir
@Inherited     // Alt sınıflar inherit eder
@Repeatable    // Aynı element'e birden fazla kez uygulanabilir
```

### Annotation Processing
```java
import java.lang.reflect.*;

public class AnnotationProcessor {
    public static void processClass(Class<?> clazz) {
        // Class-level annotations
        if (clazz.isAnnotationPresent(APIEndpoint.class)) {
            APIEndpoint endpoint = clazz.getAnnotation(APIEndpoint.class);
            System.out.println("Class endpoint: " + endpoint.path());
        }
        
        // Method-level annotations
        for (Method method : clazz.getDeclaredMethods()) {
            if (method.isAnnotationPresent(Test.class)) {
                Test test = method.getAnnotation(Test.class);
                System.out.println("Test metodu: " + method.getName() 
                                 + ", timeout: " + test.timeout());
                
                try {
                    method.invoke(clazz.newInstance());
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            
            if (method.isAnnotationPresent(APIEndpoint.class)) {
                APIEndpoint endpoint = method.getAnnotation(APIEndpoint.class);
                System.out.println("Method endpoint: " + endpoint.path() 
                                 + " [" + endpoint.method() + "]");
            }
        }
    }
    
    public static void main(String[] args) {
        processClass(TestSinifi.class);
        processClass(UserController.class);
    }
}
```

### Functional Interface Annotation
```java
@FunctionalInterface
public interface MatematikscelIslem {
    double hesapla(double a, double b);
    
    // Default ve static metotlar sayılmaz
    default void log() {
        System.out.println("İşlem yapılıyor");
    }
    
    static void info() {
        System.out.println("Matematiksel işlem interface'i");
    }
}

// Lambda ile kullanım
MatematikscelIslem toplama = (a, b) -> a + b;
MatematikscelIslem carpma = (a, b) -> a * b;

double sonuc1 = toplama.hesapla(5, 3); // 8.0
double sonuc2 = carpma.hesapla(4, 7);  // 28.0
```

---

## Enum Türleri

### Basit Enum
```java
public enum Gun {
    PAZARTESI, SALI, CARSAMBA, PERSEMBE, CUMA, CUMARTESI, PAZAR
}

// Kullanım
Gun bugun = Gun.PAZARTESI;
System.out.println(bugun); // PAZARTESI

// String'e dönüştürme
String gunAdi = bugun.name(); // "PAZARTESI"
String gunAdi2 = bugun.toString(); // "PAZARTESI"

// String'den enum'a dönüştürme
Gun gun = Gun.valueOf("SALI"); // Gun.SALI

// Tüm değerleri alma
Gun[] tumGunler = Gun.values();
for (Gun g : tumGunler) {
    System.out.println(g);
}

// Sıralama
int sira = bugun.ordinal(); // 0 (PAZARTESI ilk eleman)

// Karşılaştırma
boolean esitMi = (bugun == Gun.PAZARTESI); // true
int karsilastir = bugun.compareTo(Gun.CUMA); // negatif değer
```

### Gelişmiş Enum (Fields ve Methods ile)
```java
public enum Gezegen {
    MERKUR(3.303e+23, 2.4397e6),
    VENUS(4.869e+24, 6.0518e6),
    DUNYA(5.976e+24, 6.37814e6),
    MARS(6.421e+23, 3.3972e6);
    
    private final double kutle;   // kg cinsinden
    private final double yaricap; // metre cinsinden
    
    // Enum constructor (private)
    Gezegen(double kutle, double yaricap) {
        this.kutle = kutle;
        this.yaricap = yaricap;
    }
    
    // Getter metotları
    public double getKutle() { return kutle; }
    public double getYaricap() { return yaricap; }
    
    // Universal gravitational constant  (m3 kg-1 s-2)
    public static final double G = 6.67300E-11;
    
    // Yüzey ağırlığını hesapla
    public double yuzeyAgirligi(double digerKutle) {
        return digerKutle * G * kutle / (yaricap * yaricap);
    }
    
    // Override toString
    @Override
    public String toString() {
        return name().charAt(0) + name().substring(1).toLowerCase();
    }
}

// Kullanım
Gezegen dunya = Gezegen.DUNYA;
double dunyadaAgirligi = dunya.yuzeyAgirligi(70); // 70 kg'lık kişinin Dünya'daki ağırlığı

System.out.println("70 kg'lık kişinin farklı gezegenlerdeki ağırlığı:");
for (Gezegen g : Gezegen.values()) {
    System.out.printf("%s: %.2f N%n", g, g.yuzeyAgirligi(70));
}
```

### Enum ile Interface
```java
public interface Islem {
    double hesapla(double x, double y);
}

public enum TemelIslem implements Islem {
    TOPLAMA("+") {
        @Override
        public double hesapla(double x, double y) { return x + y; }
    },
    CIKARMA("-") {
        @Override
        public double hesapla(double x, double y) { return x - y; }
    },
    CARPMA("*") {
        @Override
        public double hesapla(double x, double y) { return x * y; }
    },
    BOLME("/") {
        @Override
        public double hesapla(double x, double y) { return x / y; }
    };
    
    private final String sembol;
    
    TemelIslem(String sembol) {
        this.sembol = sembol;
    }
    
    public String getSembol() { return sembol; }
    
    @Override
    public String toString() { return sembol; }
}

// Kullanım
TemelIslem islem = TemelIslem.TOPLAMA;
double sonuc = islem.hesapla(5, 3); // 8.0
System.out.println("5 " + islem + " 3 = " + sonuc);
```

### EnumSet ve EnumMap
```java
import java.util.*;

// EnumSet - Enum değerleri için optimize edilmiş Set
EnumSet<Gun> haficiGunler = EnumSet.range(Gun.PAZARTESI, Gun.CUMA);
EnumSet<Gun> haftasonu = EnumSet.of(Gun.CUMARTESI, Gun.PAZAR);
EnumSet<Gun> tumGunler = EnumSet.allOf(Gun.class);
EnumSet<Gun> hicGun = EnumSet.noneOf(Gun.class);

// EnumMap - Enum anahtarları için optimize edilmiş Map
EnumMap<Gun, String> gunPlani = new EnumMap<>(Gun.class);
gunPlani.put(Gun.PAZARTESI, "Toplantı");
gunPlani.put(Gun.SALI, "Proje çalışması");
gunPlani.put(Gun.CUMA, "Sunum");

for (Map.Entry<Gun, String> entry : gunPlani.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

---

## File I/O ve NIO

### Geleneksel File I/O

#### File Okuma
```java
import java.io.*;

public class FileOkuma {
    // FileReader ve BufferedReader ile
    public void dosyaOku(String dosyaYolu) {
        try (FileReader fileReader = new FileReader(dosyaYolu);
             BufferedReader bufferedReader = new BufferedReader(fileReader)) {
            
            String satir;
            while ((satir = bufferedReader.readLine()) != null) {
                System.out.println(satir);
            }
            
        } catch (IOException e) {
            System.err.println("Dosya okuma hatası: " + e.getMessage());
        }
    }
    
    // FileInputStream ile binary okuma
    public void binaryOku(String dosyaYolu) {
        try (FileInputStream fis = new FileInputStream(dosyaYolu)) {
            int veri;
            while ((veri = fis.read()) != -1) {
                System.out.print((char) veri);
            }
        } catch (IOException e) {
            System.err.println("Binary dosya okuma hatası: " + e.getMessage());
        }
    }
}
```

#### File Yazma
```java
public class FileYazma {
    // FileWriter ve BufferedWriter ile
    public void dosyaYaz(String dosyaYolu, String icerik) {
        try (FileWriter fileWriter = new FileWriter(dosyaYolu);
             BufferedWriter bufferedWriter = new BufferedWriter(fileWriter)) {
            
            bufferedWriter.write(icerik);
            bufferedWriter.newLine();
            bufferedWriter.write("İkinci satır");
            
        } catch (IOException e) {
            System.err.println("Dosya yazma hatası: " + e.getMessage());
        }
    }
    
    // Append mode ile ekleme
    public void dosyayaEkle(String dosyaYolu, String icerik) {
        try (FileWriter fileWriter = new FileWriter(dosyaYolu, true)) {
            fileWriter.write(icerik + "\n");
        } catch (IOException e) {
            System.err.println("Dosya ekleme hatası: " + e.getMessage());
        }
    }
    
    // PrintWriter ile kolay yazma
    public void printWriterIle(String dosyaYolu) {
        try (PrintWriter writer = new PrintWriter(new FileWriter(dosyaYolu))) {
            writer.println("Merhaba Dünya");
            writer.printf("Sayı: %d, Ondalık: %.2f%n", 42, 3.14159);
        } catch (IOException e) {
            System.err.println("PrintWriter hatası: " + e.getMessage());
        }
    }
}
```

### NIO.2 (Java 7+)

#### Path ve Files Kullanımı
```java
import java.nio.file.*;
import java.nio.charset.StandardCharsets;
import java.util.List;

public class NIOOrnekleri {
    public void pathOrnekleri() {
        // Path oluşturma
        Path dosya = Paths.get("test.txt");
        Path klasor = Paths.get("/home/user/documents");
        Path birlesik = Paths.get("/home", "user", "documents", "file.txt");
        
        // Path bilgileri
        System.out.println("Dosya adı: " + dosya.getFileName());
        System.out.println("Parent: " + dosya.getParent());
        System.out.println("Mutlak yol: " + dosya.toAbsolutePath());
        System.out.println("Normalize: " + Paths.get("/home/../home/user").normalize());
    }
    
    public void filesOrnekleri() throws IOException {
        Path dosyaYolu = Paths.get("ornek.txt");
        
        // Dosya varlık kontrolü
        if (Files.exists(dosyaYolu)) {
            System.out.println("Dosya mevcut");
        }
        
        // Dosya özellikleri
        System.out.println("Dosya mı: " + Files.isRegularFile(dosyaYolu));
        System.out.println("Klasör mü: " + Files.isDirectory(dosyaYolu));
        System.out.println("Boyut: " + Files.size(dosyaYolu) + " bytes");
        
        // Basit okuma/yazma
        String icerik = "Merhaba NIO.2!";
        Files.write(dosyaYolu, icerik.getBytes(StandardCharsets.UTF_8));
        
        List<String> satirlar = Files.readAllLines(dosyaYolu, StandardCharsets.UTF_8);
        satirlar.forEach(System.out::println);
        
        // Tek seferde tüm içeriği okuma
        String tumIcerik = Files.readString(dosyaYolu); // Java 11+
        System.out.println(tumIcerik);
    }
    
    public void klasorIslemleri() throws IOException {
        Path yeniKlasor = Paths.get("yeni_klasor");
        
        // Klasör oluştur
        Files.createDirectory(yeniKlasor);
        
        // Alt klasörlerle birlikte oluştur
        Files.createDirectories(Paths.get("a/b/c/d"));
        
        // Dosya kopyala
        Path kaynak = Paths.get("kaynak.txt");
        Path hedef = Paths.get("hedef.txt");
        Files.copy(kaynak, hedef, StandardCopyOption.REPLACE_EXISTING);
        
        // Dosya taşı
        Files.move(hedef, Paths.get("yeni_konum.txt"), StandardCopyOption.REPLACE_EXISTING);
        
        // Dosya sil
        Files.deleteIfExists(Paths.get("yeni_konum.txt"));
    }
}
```

#### Stream API ile File İşlemleri
```java
import java.nio.file.*;
import java.util.stream.Stream;

public class FileStreams {
    public void streamOrnekleri() throws IOException {
        Path klasor = Paths.get(".");
        
        // Klasördeki dosyaları listele
        try (Stream<Path> dosyalar = Files.list(klasor)) {
            dosyalar.filter(Files::isRegularFile)
                   .forEach(System.out::println);
        }
        
        // Rekürsif olarak tüm dosyaları bul
        try (Stream<Path> tumDosyalar = Files.walk(klasor)) {
            tumDosyalar.filter(p -> p.toString().endsWith(".java"))
                      .forEach(System.out::println);
        }
        
        // Dosya içeriğini satır satır oku
        try (Stream<String> satirlar = Files.lines(Paths.get("test.txt"))) {
            satirlar.filter(satir -> satir.contains("Java"))
                   .forEach(System.out::println);
        }
    }
}
```

---

## Inner Classes

### Static Nested Classes
```java
public class DisSinif {
    private static int staticDeger = 100;
    private int instanceDeger = 200;
    
    // Static nested class
    public static class StaticNestedSinif {
        public void metot() {
            System.out.println("Static değer: " + staticDeger); // OK
            // System.out.println(instanceDeger); // HATA: instance field'a erişemez
        }
    }
}

// Kullanım
DisSinif.StaticNestedSinif nested = new DisSinif.StaticNestedSinif();
nested.metot();
```

### Non-static Inner Classes
```java
public class DisSinif {
    private int disDeger = 100;
    
    // Inner class (non-static)
    public class IcSinif {
        private int icDeger = 200;
        
        public void goster() {
            System.out.println("Dış değer: " + disDeger); // Dış sınıfa erişim
            System.out.println("İç değer: " + icDeger);
            System.out.println("Dış sınıf referansı: " + DisSinif.this);
        }
    }
    
    public void innerOlustur() {
        IcSinif ic = new IcSinif(); // Dış sınıfın içinden
        ic.goster();
    }
}

// Kullanım
DisSinif dis = new DisSinif();
DisSinif.IcSinif ic = dis.new IcSinif(); // Dış instance gerekli
ic.goster();
```

### Local Classes
```java
public class LocalClassOrnek {
    private int disAlan = 100;
    
    public void metot() {
        final int lokalDegisken = 300; // Effectively final olmalı
        int digerDegisken = 400;       // Effectively final
        
        // Local class
        class LokalSinif {
            public void yazdir() {
                System.out.println("Dış alan: " + disAlan);
                System.out.println("Lokal değişken: " + lokalDegisken);
                System.out.println("Diğer değişken: " + digerDegisken);
                // digerDegisken = 500; // Eğer bu satır olsaydı effectively final olmazdı
            }
        }
        
        LokalSinif lokal = new LokalSinif();
        lokal.yazdir();
    }
}
```

### Anonymous Classes
```java
public class AnonymousOrnek {
    public void ornekler() {
        // Interface implementation
        Runnable task = new Runnable() {
            @Override
            public void run() {
                System.out.println("Anonymous Runnable çalışıyor");
            }
        };
        
        // Abstract class extension
        abstract class AbstractSinif {
            abstract void metot();
        }
        
        AbstractSinif obj = new AbstractSinif() {
            @Override
            void metot() {
                System.out.println("Anonymous abstract implementation");
            }
        };
        
        // Event handling örneği
        ActionListener listener = new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println("Button clicked!");
            }
        };
    }
    
    // Comparator ile anonymous class
    public void sortOrnek() {
        List<String> isimler = Arrays.asList("Zeynep", "Ali", "Mehmet");
        
        Collections.sort(isimler, new Comparator<String>() {
            @Override
            public int compare(String s1, String s2) {
                return s1.length() - s2.length(); // Uzunluğa göre sırala
            }
        });
        
        // Lambda ile aynı işlem
        Collections.sort(isimler, (s1, s2) -> s1.length() - s2.length());
    }
}
```

---

## equals, hashCode ve Comparisons

### Object Contract: equals ve hashCode

#### equals() Metodu
```java
public class Kisi {
    private String ad;
    private int yas;
    private String email;
    
    public Kisi(String ad, int yas, String email) {
        this.ad = ad;
        this.yas = yas;
        this.email = email;
    }
    
    @Override
    public boolean equals(Object obj) {
        // 1. Referans kontrolü
        if (this == obj) return true;
        
        // 2. null kontrolü
        if (obj == null) return false;
        
        // 3. Sınıf kontrolü
        if (getClass() != obj.getClass()) return false;
        
        // 4. Alanları karşılaştır
        Kisi other = (Kisi) obj;
        return yas == other.yas &&
               Objects.equals(ad, other.ad) &&
               Objects.equals(email, other.email);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(ad, yas, email);
    }
    
    // toString da ekleyelim
    @Override
    public String toString() {
        return String.format("Kisi{ad='%s', yas=%d, email='%s'}", ad, yas, email);
    }
}

// Kullanım
Kisi kisi1 = new Kisi("Ali", 25, "ali@email.com");
Kisi kisi2 = new Kisi("Ali", 25, "ali@email.com");
System.out.println(kisi1.equals(kisi2)); // true
System.out.println(kisi1.hashCode() == kisi2.hashCode()); // true (genellikle)
```

### Comparable Interface
```java
public class Ogrenci implements Comparable<Ogrenci> {
    private String ad;
    private double not;
    
    public Ogrenci(String ad, double not) {
        this.ad = ad;
        this.not = not;
    }
    
    @Override
    public int compareTo(Ogrenci other) {
        // Nota göre sıralama (yüksekten düşüğe)
        return Double.compare(other.not, this.not);
        
        // Veya multiple criteria
        // int notKarsilastir = Double.compare(other.not, this.not);
        // return notKarsilastir != 0 ? notKarsilastir : this.ad.compareTo(other.ad);
    }
    
    // equals ve hashCode da implement etmek iyi pratik
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Ogrenci ogrenci = (Ogrenci) obj;
        return Double.compare(ogrenci.not, not) == 0 && Objects.equals(ad, ogrenci.ad);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(ad, not);
    }
    
    @Override
    public String toString() {
        return String.format("%s (%.1f)", ad, not);
    }
}

// Kullanım
List<Ogrenci> ogrenciler = Arrays.asList(
    new Ogrenci("Ali", 85.5),
    new Ogrenci("Ayşe", 92.0),
    new Ogrenci("Mehmet", 78.5)
);

Collections.sort(ogrenciler); // Comparable kullanarak sıralar
ogrenciler.forEach(System.out::println);
```

### Comparator Interface
```java
import java.util.Comparator;

public class ComparatorOrnekleri {
    public void ornekler() {
        List<Kisi> kisiler = Arrays.asList(
            new Kisi("Zeynep", 28, "zeynep@email.com"),
            new Kisi("Ali", 25, "ali@email.com"),
            new Kisi("Mehmet", 30, "mehmet@email.com")
        );
        
        // Yaşa göre sıralama
        Collections.sort(kisiler, new Comparator<Kisi>() {
            @Override
            public int compare(Kisi k1, Kisi k2) {
                return Integer.compare(k1.getYas(), k2.getYas());
            }
        });
        
        // Lambda ile aynı işlem
        Collections.sort(kisiler, (k1, k2) -> Integer.compare(k1.getYas(), k2.getYas()));
        
        // Method reference ile
        Collections.sort(kisiler, Comparator.comparing(Kisi::getYas));
        
        // Ters sıralama
        Collections.sort(kisiler, Comparator.comparing(Kisi::getYas).reversed());
        
        // Çoklu kriter sıralama
        Collections.sort(kisiler, 
            Comparator.comparing(Kisi::getYas)
                     .thenComparing(Kisi::getAd));
        
        // Stream ile sıralama
        List<Kisi> sirali = kisiler.stream()
            .sorted(Comparator.comparing(Kisi::getAd))
            .collect(Collectors.toList());
    }
}
```

---

## Regular Expressions

### Pattern ve Matcher
```java
import java.util.regex.*;

public class RegexOrnekleri {
    public void temelKullanim() {
        String metin = "Java programlama dili çok güçlü. Java 17 harika!";
        String pattern = "Java";
        
        // Pattern ve Matcher kullanımı
        Pattern p = Pattern.compile(pattern);
        Matcher m = p.matcher(metin);
        
        // Tüm eşleşmeleri bul
        while (m.find()) {
            System.out.println("Bulundu: " + m.group() + 
                             " [" + m.start() + "-" + m.end() + "]");
        }
        
        // String sınıfı regex metotları
        boolean eslesiyorMu = metin.matches(".*Java.*"); // true
        String[] parcalar = metin.split("\\s+"); // Boşluklara göre böl
        String degistirilmis = metin.replaceAll("Java", "Python");
        
        System.out.println("Eşleşiyor: " + eslesiyorMu);
        System.out.println("Parçalar: " + Arrays.toString(parcalar));
        System.out.println("Değiştirilmiş: " + degistirilmis);
    }
    
    public void gelismisOrnekler() {
        // Email validation
        String emailPattern = "^[A-Za-z0-9+_.-]+@([A-Za-z0-9.-]+\\.[A-Za-z]{2,})$";
        Pattern emailRegex = Pattern.compile(emailPattern);
        
        String[] emails = {"test@example.com", "invalid.email", "user@domain.co.uk"};
        for (String email : emails) {
            if (emailRegex.matcher(email).matches()) {
                System.out.println(email + " geçerli");
            } else {
                System.out.println(email + " geçersiz");
            }
        }
        
        // Telefon numarası formatı
        String telefonPattern = "\\+?[0-9]{1,3}[-\\s]?\\(?[0-9]{3}\\)?[-\\s]?[0-9]{3}[-\\s]?[0-9]{4}";
        String telefon = "+90 (532) 123-4567";
        if (telefon.matches(telefonPattern)) {
            System.out.println("Geçerli telefon numarası");
        }
        
        // Gruplar (Capturing Groups)
        String tarihPattern = "(\\d{2})/(\\d{2})/(\\d{4})";
        Pattern tarihRegex = Pattern.compile(tarihPattern);
        Matcher tarihMatcher = tarihRegex.matcher("15/03/2024");
        
        if (tarihMatcher.matches()) {
            System.out.println("Gün: " + tarihMatcher.group(1));    // 15
            System.out.println("Ay: " + tarihMatcher.group(2));     // 03
            System.out.println("Yıl: " + tarihMatcher.group(3));    // 2024
            System.out.println("Tam: " + tarihMatcher.group(0));    // 15/03/2024
        }
    }
    
    public void yaygınPatternler() {
        // Yaygın regex pattern'ları
        Map<String, String> patterns = new HashMap<>();
        patterns.put("Sadece rakamlar", "\\d+");
        patterns.put("Sadece harfler", "[a-zA-Z]+");
        patterns.put("Alphanumeric", "[a-zA-Z0-9]+");
        patterns.put("Boşluk karakterler", "\\s+");
        patterns.put("Email", "[\\w\\._%+-]+@[\\w\\.-]+\\.[A-Za-z]{2,}");
        patterns.put("URL", "https?://[\\w\\.-]+(?:/[\\w\\._~:/?#[\\]@!$&'()*+,;=-]*)?");
        patterns.put("IP Adresi", "\\b(?:[0-9]{1,3}\\.){3}[0-9]{1,3}\\b");
        patterns.put("Kredi Kartı", "\\d{4}[-\\s]?\\d{4}[-\\s]?\\d{4}[-\\s]?\\d{4}");
        
        String testMetni = "Email: test@example.com, IP: 192.168.1.1, URL: https://www.example.com";
        
        patterns.forEach((isim, pattern) -> {
            Pattern p = Pattern.compile(pattern);
            Matcher m = p.matcher(testMetni);
            if (m.find()) {
                System.out.println(isim + " bulundu: " + m.group());
            }
        });
    }
}
```

---

## System ve JVM

### System Sınıfı
```java
public class SystemOrnekleri {
    public void systemMetotları() {
        // Sistem özellikleri
        System.out.println("Java Versiyonu: " + System.getProperty("java.version"));
        System.out.println("İşletim Sistemi: " + System.getProperty("os.name"));
        System.out.println("Kullanıcı Adı: " + System.getProperty("user.name"));
        System.out.println("Çalışma Dizini: " + System.getProperty("user.dir"));
        System.out.println("Java Home: " + System.getProperty("java.home"));
        System.out.println("Classpath: " + System.getProperty("java.class.path"));
        
        // Çevresel değişkenler
        Map<String, String> env = System.getenv();
        System.out.println("PATH: " + env.get("PATH"));
        System.out.println("HOME: " + env.get("HOME"));
        
        // Zaman ölçümü
        long startTime = System.currentTimeMillis();
        // Bazı işlemler...
        long endTime = System.currentTimeMillis();
        System.out.println("İşlem süresi: " + (endTime - startTime) + " ms");
        
        // Nanosaniye hassasiyetiyle
        long nanoStart = System.nanoTime();
        // İşlemler...
        long nanoEnd = System.nanoTime();
        System.out.println("Nano süre: " + (nanoEnd - nanoStart) + " ns");
        
        // Sistem çıkışı
        // System.exit(0); // JVM'i sonlandır
    }
    
    public void arrayKopyalama() {
        // Etkili array kopyalama
        int[] kaynak = {1, 2, 3, 4, 5};
        int[] hedef = new int[5];
        
        System.arraycopy(kaynak, 0, hedef, 0, kaynak.length);
        System.out.println("Kopyalanan: " + Arrays.toString(hedef));
        
        // Partial kopyalama
        int[] partial = new int[3];
        System.arraycopy(kaynak, 1, partial, 0, 3); // [2, 3, 4]
        System.out.println("Kısmi kopya: " + Arrays.toString(partial));
    }
}
```

### Runtime ve Memory Management
```java
public class RuntimeOrnekleri {
    public void runtimeBilgileri() {
        Runtime runtime = Runtime.getRuntime();
        
        // Memory bilgileri
        long totalMemory = runtime.totalMemory();
        long freeMemory = runtime.freeMemory();
        long maxMemory = runtime.maxMemory();
        long usedMemory = totalMemory - freeMemory;
        
        System.out.println("Toplam Memory: " + (totalMemory / 1024 / 1024) + " MB");
        System.out.println("Kullanılan Memory: " + (usedMemory / 1024 / 1024) + " MB");
        System.out.println("Serbest Memory: " + (freeMemory / 1024 / 1024) + " MB");
        System.out.println("Max Memory: " + (maxMemory / 1024 / 1024) + " MB");
        
        // Processor bilgisi
        int processors = runtime.availableProcessors();
        System.out.println("İşlemci Sayısı: " + processors);
        
        // Garbage Collection öner
        runtime.gc();
        System.out.println("GC çalıştırıldı");
        
        // Shutdown hook ekle
        runtime.addShutdownHook(new Thread(() -> {
            System.out.println("Uygulama sonlandırılıyor...");
            // Cleanup işlemleri
        }));
    }
    
    public void processKomutları() throws IOException {
        // Dış komut çalıştırma
        Process process = Runtime.getRuntime().exec("java -version");
        
        // Process output okuma
        try (BufferedReader reader = new BufferedReader(
                new InputStreamReader(process.getErrorStream()))) {
            
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        }
        
        // Process'in bitmesini bekle
        try {
            int exitCode = process.waitFor();
            System.out.println("Exit code: " + exitCode);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

### JVM Temel Bilgiler
```java
public class JVMBilgileri {
    public void jvmBilgileri() {
        // JVM bilgileri
        System.out.println("=== JVM Bilgileri ===");
        System.out.println("Java Vendor: " + System.getProperty("java.vendor"));
        System.out.println("JVM Name: " + System.getProperty("java.vm.name"));
        System.out.println("JVM Version: " + System.getProperty("java.vm.version"));
        System.out.println("JVM Vendor: " + System.getProperty("java.vm.vendor"));
        
        // Memory areas
        MemoryMXBean memoryBean = ManagementFactory.getMemoryMXBean();
        MemoryUsage heapUsage = memoryBean.getHeapMemoryUsage();
        MemoryUsage nonHeapUsage = memoryBean.getNonHeapMemoryUsage();
        
        System.out.println("\n=== Memory Usage ===");
        System.out.println("Heap Used: " + (heapUsage.getUsed() / 1024 / 1024) + " MB");
        System.out.println("Heap Max: " + (heapUsage.getMax() / 1024 / 1024) + " MB");
        System.out.println("Non-Heap Used: " + (nonHeapUsage.getUsed() / 1024 / 1024) + " MB");
        
        // Garbage Collection bilgileri
        List<GarbageCollectorMXBean> gcBeans = ManagementFactory.getGarbageCollectorMXBeans();
        System.out.println("\n=== GC Bilgileri ===");
        for (GarbageCollectorMXBean gcBean : gcBeans) {
            System.out.println("GC Name: " + gcBean.getName());
            System.out.println("Collection Count: " + gcBean.getCollectionCount());
            System.out.println("Collection Time: " + gcBean.getCollectionTime() + " ms");
        }
    }
    
    public void memoryLeak() {
        // Memory leak örneği (YAPMAIN!)
        List<Object> list = new ArrayList<>();
        while (true) {
            list.add(new Object()); // Sonsuza kadar nesne ekler
            // Bu kod OutOfMemoryError'a sebep olur
        }
    }
}
```

---

## Paketler ve Sık Kullanılan Sınıflar

### Paketler (package) ve import

#### Paket Tanımlama
```java
// Dosyanın en üstünde
package com.sirket.uygulama.model;

import java.util.List;
import java.util.ArrayList;
import java.time.LocalDate;
import java.io.*; // Tüm java.io paketini import et

// Static import
import static java.lang.Math.*;
import static java.util.Collections.*;

public class Ornek {
    public void metot() {
        // Math.PI yerine direkt PI kullanabiliriz
        double alan = PI * pow(r, 2);
        
        // Collections.sort yerine direkt sort
        List<String> liste = new ArrayList<>();
        sort(liste);
    }
}
```

### String Sınıfı Metotları

```java
public class StringOrnekleri {
    public void stringMetotlari() {
        String metin = "Merhaba Java Dünyası";
        
        // Temel özellikler
        int uzunluk = metin.length();                    // 20
        boolean bosmu = metin.isEmpty();                 // false
        boolean bosMusVeyaBosluk = metin.isBlank();      // false (Java 11+)
        
        // Karakter erişimi
        char ilkKarakter = metin.charAt(0);              // 'M'
        int indexBul = metin.indexOf("Java");            // 8
        int sonIndexBul = metin.lastIndexOf("a");        // 18
        
        // Alt string
        String parcala = metin.substring(8);             // "Java Dünyası"
        String parcala2 = metin.substring(8, 12);        // "Java"
        
        // Karşılaştırma
        boolean esitMi = metin.equals("Merhaba Java Dünyası");      // true
        boolean esitMiBuyukKucuk = metin.equalsIgnoreCase("MERHABA java dünyası"); // true
        int karsilastir = metin.compareTo("Merhaba");    // pozitif değer
        
        // Arama ve kontrol
        boolean iceriyorMu = metin.contains("Java");     // true
        boolean baslarMi = metin.startsWith("Mer");      // true
        boolean bitiyorMu = metin.endsWith("ası");       // true
        
        // Değiştirme
        String degistir = metin.replace("Java", "Python");        // "Merhaba Python Dünyası"
        String regexDegistir = metin.replaceAll("\\s+", "-");     // "Merhaba-Java-Dünyası"
        String ilkDegistir = metin.replaceFirst("a", "@");        // "Merh@ba Java Dünyası"
        
        // Büyük/küçük harf
        String buyukHarf = metin.toUpperCase();          // "MERHABA JAVA DÜNYASI"
        String kucukHarf = metin.toLowerCase();          // "merhaba java dünyası"
        
        // Bölme
        String[] kelimeler = metin.split(" ");           // ["Merhaba", "Java", "Dünyası"]
        String[] sinirliBolem = metin.split(" ", 2);     // ["Merhaba", "Java Dünyası"]
        
        // Trim (boşluk temizleme)
        String bosluklu = "  Java  ";
        String temizlenmis = bosluklu.trim();            // "Java"
        String stripEdilmis = bosluklu.strip();          // "Java" (Java 11+, Unicode aware)
        
        // Format
        String formatli = String.format("Merhaba %s! Yaşınız %d.", "Ali", 25);
        
        System.out.println("Uzunluk: " + uzunluk);
        System.out.println("Alt string: " + parcala);
        System.out.println("Büyük harf: " + buyukHarf);
    }
}
```

### StringBuilder Sınıfı

```java
public class StringBuilderOrnegi {
    public void performansliBirlestirme() {
        // Verimsiz yöntem (String immutable)
        String sonuc1 = "";
        for (int i = 0; i < 1000; i++) {
            sonuc1 += "Sayı: " + i + " ";  // Her seferinde yeni string oluşturur
        }
        
        // Verimli yöntem (StringBuilder mutable)
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 1000; i++) {
            sb.append("Sayı: ").append(i).append(" ");
        }
        String sonuc2 = sb.toString();
        
        // StringBuilder metotları
        StringBuilder builder = new StringBuilder("Merhaba");
        builder.append(" Dünya");                    // "Merhaba Dünya"
        builder.insert(8, "Java ");                  // "Merhaba Java Dünya"
        builder.delete(8, 13);                       // "Merhaba Dünya"
        builder.reverse();                           // "aynüD abahreM"
        builder.reverse();                           // "Merhaba Dünya"
        
        // Kapasite yönetimi
        builder.ensureCapacity(50);                  // Minimum kapasite ayarla
        int kapasite = builder.capacity();           // Mevcut kapasite
        builder.trimToSize();                        // Gereksiz kapasiteyi temizle
        
        System.out.println("Sonuç: " + builder.toString());
    }
}
```

### Sarmalayıcı Sınıflar (Wrapper Classes)

```java
public class WrapperSiniflar {
    public void wrapperOrnekleri() {
        // Auto-boxing (primitive -> wrapper)
        Integer sayi1 = 42;                          // int -> Integer
        Double ondalik1 = 3.14;                      // double -> Double
        Boolean dogru1 = true;                       // boolean -> Boolean
        
        // Explicit boxing
        Integer sayi2 = Integer.valueOf(42);
        Double ondalik2 = Double.valueOf(3.14);
        
        // Auto-unboxing (wrapper -> primitive)
        int primitifSayi = sayi1;                    // Integer -> int
        double primitifOndalik = ondalik1;           // Double -> double
        
        // String'den dönüşüm
        int intDeger = Integer.parseInt("123");      // "123" -> 123
        double doubleDeger = Double.parseDouble("3.14"); // "3.14" -> 3.14
        boolean boolDeger = Boolean.parseBoolean("true"); // "true" -> true
        
        // valueOf metotları (caching avantajı)
        Integer cached1 = Integer.valueOf(100);      // Cache'den alır (-128 to 127)
        Integer cached2 = Integer.valueOf(100);      // Aynı referans
        System.out.println(cached1 == cached2);     // true
        
        // Büyük sayılar için yeni nesne
        Integer large1 = Integer.valueOf(1000);
        Integer large2 = Integer.valueOf(1000);
        System.out.println(large1 == large2);       // false (farklı nesneler)
        System.out.println(large1.equals(large2));  // true (değer eşitliği)
        
        // Yararlı metotlar
        System.out.println(Integer.MAX_VALUE);       // 2147483647
        System.out.println(Integer.MIN_VALUE);       // -2147483648
        System.out.println(Integer.bitCount(7));     // 3 (binary'de 1'lerin sayısı)
        System.out.println(Integer.toBinaryString(7)); // "111"
        System.out.println(Integer.toHexString(255)); // "ff"
        
        // Karşılaştırma
        System.out.println(Integer.compare(5, 10));  // -1 (5 < 10)
        System.out.println(Double.compare(3.14, 2.71)); // 1 (3.14 > 2.71)
    }
}
```

### Math Sınıfı

```java
public class MathOrnekleri {
    public void mathMetotlari() {
        // Temel matematiksel işlemler
        double mutlakDeger = Math.abs(-5.7);         // 5.7
        double usAlma = Math.pow(2, 3);              // 8.0 (2^3)
        double karekok = Math.sqrt(16);              // 4.0
        double kupkok = Math.cbrt(27);               // 3.0
        
        // Yuvarlama
        double yukariya = Math.ceil(4.3);            // 5.0
        double asagiya = Math.floor(4.8);            // 4.0
        long yuvarlama = Math.round(4.7);            // 5
        
        // Min/Max
        int minDeger = Math.min(10, 20);             // 10
        double maxDeger = Math.max(15.5, 12.8);      // 15.5
        
        // Trigonometrik fonksiyonlar (radyan cinsinden)
        double sinus = Math.sin(Math.PI / 2);        // 1.0 (sin 90°)
        double cosinus = Math.cos(Math.PI);          // -1.0 (cos 180°)
        double tanjant = Math.tan(Math.PI / 4);      // 1.0 (tan 45°)
        
        // Derece-radyan dönüşümü
        double radyan = Math.toRadians(90);          // π/2
        double derece = Math.toDegrees(Math.PI);     // 180.0
        
        // Logaritma
        double dogalLog = Math.log(Math.E);          // 1.0 (ln e)
        double log10 = Math.log10(100);              // 2.0 (log₁₀ 100)
        
        // Rastgele sayı
        double rastgele = Math.random();             // 0.0-1.0 arası
        int rastgeleTam = (int)(Math.random() * 10); // 0-9 arası tam sayı
        
        // Sabitler
        System.out.println("PI: " + Math.PI);       // 3.141592653589793
        System.out.println("E: " + Math.E);         // 2.718281828459045
        
        // Daha karmaşık işlemler
        double hipotenus = Math.hypot(3, 4);         // 5.0 (sqrt(3²+4²))
        double exp = Math.exp(1);                    // 2.718... (e^1)
        
        System.out.println("Karekok 16: " + karekok);
        System.out.println("Rastgele sayı: " + rastgele);
    }
}
```

### Diğer Önemli Sınıflar

#### Random Sınıfı
```java
import java.util.Random;

Random random = new Random();
int rastgeleInt = random.nextInt(100);        // 0-99 arası
double rastgeleDouble = random.nextDouble();  // 0.0-1.0 arası
boolean rastgeleBool = random.nextBoolean();  // true/false

// Seed ile (tekrarlanabilir rastgelelik)
Random seedRandom = new Random(12345);
```

#### Scanner Sınıfı (Kullanıcı Girdisi)
```java
import java.util.Scanner;

Scanner scanner = new Scanner(System.in);
System.out.print("Adınız: ");
String ad = scanner.nextLine();
System.out.print("Yaşınız: ");
int yas = scanner.nextInt();

scanner.close(); // Kaynağı kapat
```

#### Arrays Utility Sınıfı
```java
import java.util.Arrays;

int[] dizi = {5, 2, 8, 1, 9};
Arrays.sort(dizi);                           // Sıralama
System.out.println(Arrays.toString(dizi));   // [1, 2, 5, 8, 9]

int[] kopya = Arrays.copyOf(dizi, dizi.length);
boolean esitMi = Arrays.equals(dizi, kopya); // true

int index = Arrays.binarySearch(dizi, 5);    // Sıralı dizide arama
Arrays.fill(dizi, 0);                        // Tüm elemanları 0 yap
```

---

Bu cheatsheet, Java programlama dilinin temel ve modern özelliklerini kapsamlı bir şekilde ele almaktadır. Her bölüm, pratik kod örnekleri ve açıklamalarla desteklenmiştir. Hızlı başvuru amaçlı kullanılabileceği gibi, Java öğreniminde de rehber olarak kullanılabilir.