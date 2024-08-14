# SICP

# İşlemlerle soyutlamalar kurmak

## Programlamanın temelleri

Güçlü her programlama dilinde bulunanlar: 

- **Temel ifadeler (expressions)**: en temeldeki işlemler (toplama, çıkarma, çarpma…)
- **Kombinasyon yöntemleri**: daha basit elementleri birleştirebilme
- **Soyutlama yöntemleri**: elemanlara isim takma ve bunları manipüle edebilme

### İfadeler

Biz bilgisayara bir ifade veririz. Bilgisayar bunu yorumlayıcı (interpreter) sayesinde hesaplayıp sana sonucu verir. Matematiksel işlemler en temel ifadelerden biridir. 

```scheme
486
;486

(+ 137 349)
;486

(- 1000 334)
;666

(* 5 99)
;495

(/ 10 5)
;2

(+ 2.7 10)
;12.7

(+ 21 35 12 7)
;75

(* 25 4 12)
;1200
```

![Untitled](SICP%208dceb8ed39974f8fb1dc0bb5d6a7bc1c/Untitled.png)

Gerçek hayattaki birçok dil gibi programlama dillerinin de yazılışında bazı kurallar vardır. Kullandığımı Scheme (ya da Lisp de denilebilir) prefix yani ön ekli bir dildir. Öncelikle yapılmasını istediğin işlemi (operator) belirtip sonrasında o işlemin neleri işleyeceğini (operand) verirsiniz. Örneğin yukarıda iki sayıyı toplamak istiyorsak öncelikle amacımızı belirten + işaretini koyduk sonrasında da hangi sayılar arasında bu işlemi gerçekleştireceğimizi belirttik.  Ön ekli dillerde iç içe (nested) ifadeler yazmak da ayrıca kolaydır. Parantezler arasındaki işlemler gerçekleştirilir, parantezin kapanmadan önce ne kadar boşluğa sahip olduğu önemli değil. Bilgisayar parantezin açıldığı ve kapandığı yerleri tespit edip arasındaki işlemleri gerçekleştirir.

```scheme
(+ (* 3 5) (- 10 6))
;19

(+ (* 3 (+ (* 2 4) (+ 3 5))) (+ (- 10 7) 6))

; daha güzel yazılmışı 
(+ (* 3
      (+ (* 2 4)
         (+ 3 5)))
   (+ (- 10 7)
      6))
```

### İsimlendirme ve Çevre

Günlük hayatta televizyonun içerisindeki karmaşıklığı bilmemize gerek kalmadan yalnızca kumandayı kullanarak kanalları değiştirebiliyoruz. Burada kumanda, televizyonun iç işleyişini gizleyerek size kanalı değiştirme işlevi sunar. İşte buna programlamada soyutlama (abtraction) deniyor. Biz de yapacağımız işlemlerin detaylarını oluşturup kullanıcıya bir kumanda vereceğiz (prosedürler). 

Bunun dışında yine günlük hayatta 18 bir sayıdır fakat tek başına bir anlam ifade etmez. Daha anlamlı olan şey 18 yaş, 18 kilo, 18 liradır. Burada anlamını değiştiren şey bizim değişkenimizdir (variable), 18 ise bizim değerimizdir (value). Programlamada da anlamlı her tür ifadeye isim verebiliyoruz. 

Bunu da (define <isim> <değer>) şeklinde yapabiliriz. Buradaki define: tanımlama anlamına gelmektedir.

```scheme
;boyut = 2
(define boyut 2)
boyut
;2

(* 5 boyut)
;10

; pi = 3.14159
(define pi 3.14159)

;yarıçap = 10
(define yarıçap 10)

;pi * (yarıçapın karesi)
(* pi (* yarıçap yarıçap))
314.159

; tek bir değer vermek yerine değişkene bir ifade(ler) de verebiliriz.
; çevre = 2 * pi * yarıçap 
(define çevre (* 2 pi yarıçap))

çevre
62.8318
```

Bu atama işlemi sonucunda bilgi hafızada isim-nesne (name-object) şeklinde tutulur. Bu bilgiyi tutan hafızaya da ortam veya çevre (environment) denir. 

### Kombinasyonları hesaplama

Matematikteki işlem sırası gibi programlamada da ifadeleri hesaplama belli bir kural ve sıraya bağlıdır. Bunlar parantez, çarpma bölme, toplama çıkarma şeklinde matematiktekine benzerdir.  

 

```scheme
(* (+ 2 (* 4 6)) (+ 3 5 7))

```

![Untitled](SICP%208dceb8ed39974f8fb1dc0bb5d6a7bc1c/Untitled%201.png)

### Bileşik prosedürler

Bilgisayar temelde hiçbir şeyi bilmez, onun bir şey yapmasını istiyorsak adım adım neler yapması gerektiğini söylememiz gerekir. Tüm adımları ona anlatma işlemi Prosedür tanımlamaları olarak adlandırılır. 

Bunu matematikteki fonksiyona benzetebiliriz çünkü oradan geliyor. 

$$
f(x) = 4x^2+2x+7
$$

 fonksiyonu bize şunu söyler: 

- Verilen sayının karesini al, 4 ile çarp, sonra bu sonucu verilen sayının 2 katıyla topla, ardından çıkan sonucu 7 ekleyerek son değeri bul.

Lisp programlama dilinde bunu şöyle yapıyoruz ve buna bileşik prosedür deniyor. 

```scheme
(define (kare x) (* x x))
; bir şeyin karesini almak için, sayıyı kendisi ile çarp 
```

Genel yapısı 

`(define (<prosedür_ismi> <gerekli_parametreler>) <yapılacak_işlemler>)`

```scheme
(define (kare x) (* x x))

(kare 21)
;441

(kare (+ 2 5))
;49

(kare (kare 3))
;81

; kare almayı öğrendik şimdi x^2+y^2 gibi bir işlem yapan prosedürü yazalım

(define (kareler-toplami x y)
	(+ (kare x) (kare y)))
	
(kareler-toplami 3 4)
;25

; bu fonksiyonu başka bir fonksiyon altında kullanalım

; bu fonksiyon verilen sayının bir fazlasının karesi ile verilen sayının iki katının karesini topluyor 

(define (f a)
  (kareler-toplami (+ a 1) (* a 2)))

(f 5) ; 6^2 + 10^2  = 36 + 100 = 136
136
```

### Prosedür Uygulamalarında Yerine Koyma (Substitution) Modeli

Burada değişkenler, ifadelerdeki yerlerine tek tek konarak ilerlenir. Örnek ile bakalım: 

 

```scheme
(define (kare x) (* x x))

(define (kareler-toplami x y)
	(+ (kare x) (kare y)))

(define (f a)
  (kareler-toplami (+ a 1) (* a 2)))
 
(f 5) ; yukarıdaki fonksiyona 5 değerini verdik
; (kareler-toplami (+a 1) (* a 2))
; (kareler-toplami (+5 1) (* 5 2))
; (kareler-toplami 6 10)
; kareler-toplami prosedürü içindeki kare prosedürüne geçtik 
; (+ (kare 6) (kare 10))
; şimdi de orijinal kare fonksiyonuna geçtik
; (kare 6) -> (* 6 6)     (kare 10) -> (* 10 10)
; (+ 36 100)
; (136)

```

Makineler de bu şekilde adım adım çalışır, (uygulayıcı-sıra hesaplaması: applicative-order evaluation). Bir prosedürün nasıl çalıştığını, ne sonuç ürettiğini analiz etmek için kullanabileceğimiz yöntemlerden biridir. 

Bir başka hesaplama yönteminde de hesaplama yapmadan önce tüm değerler yerlerine koyulur. (normal-sıra hesaplaması: normal-order evaluation)

```scheme
(kareler-toplami (+ 5 1) (* 5 2))

(+ (kare (+ 5 1)) 
   (kare (* 5 2)))

(+ (* (+ 5 1) (+ 5 1)) 
   (* (* 5 2) (* 5 2)))
   
 ; tüm değerler yerine konuldu şimdi operatörler çalışacak
 (+ (* 6 6) 
   (* 10 10))

(+ 36 100)

136
```

### Koşullu ifadeler ve yargılar

Hepimiz şöyle bir fonksiyona alışkınızdır. Bu mutlak değer fonksiyonu. “eğer ki sayı 0’dan küçükse eksi ile çarp, eğer 0’a eşit ise 0 yaz, 0’dan büyük ise olduğu gibi yaz.” anlamına gelmektedir. Bu “eğer” koşulunun programlamadaki karşılığı da koşul ifadeleridir (conditional expressions), buradaki “sayı 0’a eşit mi?” ise bizim yargımız ya da önermemiz (predicate) olarak adlandırılır.

![Untitled](SICP%208dceb8ed39974f8fb1dc0bb5d6a7bc1c/Untitled%202.png)

```scheme
(define (mutlak-deger x)
  (cond ((> x 0) x)
        ((= x 0) 0)
        ((< x 0) (- x))))
```

```
Genel kural:

(cond (⟨p₁⟩ ⟨e₁⟩)
      (⟨p₂⟩ ⟨e₂⟩)
      …
      (⟨pₙ⟩ ⟨eₙ⟩))
      
Buradaki p'ler predicateler. e ise bu koşullar sağlandığında gerçekleştirilecek işlemler (expression), ikisinin de bulunduğu paranteze de kapsam (clause) denir.
```

```scheme
; mutlak değeri yazmanın farklı bir yöntemi. 
(define (mutlak-deger x)
  (cond ((< x 0) (- x)) ; sayı 0'dan küçükse eksi ile çarp
        (else x)))      ; "diğer durumlarda" olduğu gibi yaz
        
; okunuşu kolaylaştırmak için
(define (mutlak-deger x)
  (if (< x 0)
      (- x)
      x))
```

Buradaki küçüktür, büyüktür, eşittir ifadelerinin yanı sıra AND, OR, NOT gibi mantıksal işlemler de kullanılabilir.

```lisp
"""Exercise 1.1: Below is a sequence of expressions. What is
the result printed by the interpreter in response to each expression?
Assume that the sequence is to be evaluated in
the order in which it is presented."""
10 ; 10
(+ 5 3 4) ; 12
(- 9 1) ; 8
(/ 6 2) ; 3
(+ (* 2 4) (- 4 6)) ; 6
(define a 3) ; a = 3
(define b (+ a 1)) ; b = 4
(+ a b (* a b)) ; 19
(= a b) ; False

(if (and (> b a) (< b (* a b))) ;4>3 ve 4<12 ise True
		b  ; True olduğun için sonuç 4  
		a) ; False durumu
		
(cond ((= a 4) 6) ; False
			((= b 4) (+ 6 7 a)) ;16
			(else 25)) ; yukarıdaki kod çalıştığı için burası çalışmayacak
		
(+ 2 (if (> b a) b a)) ;6

(* (cond ((> a b) a)
		((< a b) b) ;4
		(else -1))
	(+ a 1))
	; (* 4 5) = 20
```

### Example: Square Roots by Newton’s Method

Newton bir sayının karekökünü hesaplamanın bir yöntemini bulmuş. Bu yöntem şu şekilde çalışıyor:

$$
\sqrt{x} = y, y\geq 0 ,  y^2=x
$$

yani y sayısı sıfırdan farklı ise, iki tarafın da karesini alarak x’in karekökünden kurtulabiliriz. 

```scheme
(define (sqrt x)
  (the y (and (>= y 0)  ; y sıfırdan büyük veya eşit ise
              (= (square y) x)))) ; y'nin karesi x'e eşittir
```

Bu bizim kullandığımız ve bildiğimiz bir şey fakat bunu uygulayabilmek için hem x hem de y değerini bilmemiz gerekiyor. Peki ya yalnızca √***2*** gibi hesaplanmayı bekleyen bir sayımız olsaydı ne yapacaktık? 

![Untitled](SICP%208dceb8ed39974f8fb1dc0bb5d6a7bc1c/Untitled%203.png)

Bu işlemleri tekrarlayacaktık. Bu işlem ne yapıyor. 

- Öncelikle kök 2’nin sonucunun 1 olduğunu tahmin ediyor
- içerideki sayıyı (bu örnek için 2) tahminimize bölüyor ve böylelikle bölüm katsayısı elde ediliyor.
- son olarak da bölüm katsayısı ile tahminimizi toplayıp ikiye bölüyoruz yani ortalamasını alıyoruz.
- ortalamada çıkan değerimiz bizim yeni tahminimiz oluyor ve bu süreç sayının kareköküne “yeterince yakın olana kadar” devam ediyor.
    - Burada sonuç 1.4142 çıkmış ve bu da doğru sonuca ulaştık demek oluyor.

Bu prosedürün taslağını çıkaracak olursak:

```scheme
(define (sqrt-iter guess x)
  (if (good-enough? guess x) ; yeterince yakın mı?
      guess ; EVET ise, tahminimiz doğru cevap demektir.  
      (sqrt-iter (improve guess x) x))) ; HAYIR ise, tahminimizi geliştirip tekrardan prosedüre o sayı ile baştan başlıyoruz
```

Buradaki improve yani tahmini geliştirme kısmı formüldeki average yani ortalama kısmına karşılık geliyor 

```scheme
(define (improve guess x)
  (average guess (/ x guess)))
  
;(define (average x y) 
;  (/ (+ x y) 2))
```

good-enough? yani yeterince iyi mi değil mi kontrolü ise biraz bize kalmış. Biz gerçek hesaplamayı bilgisayarın kendi prosedürüne yaptırıp kendi tahminimizle karşılaştıracağız. Eğer ki aradaki fark binde 1’den azsa bu bizim için yeterli demektir. 

```scheme
(define (good-enough? guess x)
  (< (abs (- (square guess) x)) 0.001)) ;abs: mutlak değer (absolute)
```

İlk tahminimizde 1 vermiştik onu da ekleyip prosedürü toparlayalım 

```scheme
(define (sqrt x)
  (sqrt-iter 1.0 x))
  
(define (improve guess x)
  (average guess (/ x guess))) 
  
(define (good-enough? guess x)
  (< (abs (- (square guess) x)) 0.001)) 
  
  (define (sqrt-iter guess x)
  (if (good-enough? guess x) 
      guess   
      (sqrt-iter (improve guess x) x)))
      
      
(sqrt 9)
;3.00009155413138

(sqrt (+ 100 37))
;11.704699917758145

(sqrt (+ (sqrt 2) (sqrt 3)))
;1.7739279023207892

(square (sqrt 1000))
;1000.000369924366
```

### Procedures as Black-box Abstractions

![Untitled](SICP%208dceb8ed39974f8fb1dc0bb5d6a7bc1c/Untitled%204.png)

Sqrt bizim ilk prosedürlerimizden biri, gördüğümüz üzere de sqrt-iter prosedürü özyinelemeli (recursive) bir prosedür.

KELİMELER

subproblems

black-box 

how?

procedural abstraction

ÖZET

Burada problem altproblemlere ayrılıyor; tahmin yeterince iyi mi (good-enough), tahmini nasıl daha da geliştirebiliriz (improve). Her bir altproblem ayrı bir prosedür gerektiriyor. 

Biz square altında good-enough? prosedürünü tanımladığımızda square prosedürü bizim için bir kara kutu (black-box) haline gelecek. Bu andan itibaren prosedürün sonucu nasıl hesapladığını değil yalnızca square’ı hesapladığı gerçeği ile ilgileniriz. Bunun nasıl hesaplandığıyla ilgilenmeyiz. 

Burada square bir prosedür olarak düşünülmez, prosedürün soyutlanması, hayal edilmesi olarak tanımlanır. Bu duruma da prosedürel soyutlama (procedural abstraction) denir. 

```lisp
(define (square x) (* x x))
(define (square x) (exp (double (log x))))
(define (double x) (+ x x))

```

YEREL İSİMLER - LOCAL NAMES 

Prosedürün işlemleri, isminden bağımsızdır. Fakat prosedür içerisinde geçen başka prosedür isimleri yerel olarak hafızada yer almalıdır. Bizim kullandığımız abs, +, -, define gibi ifadelerin özellikle tanımlanmasına gerek yoktur zaten dilin içerisinde varsayılan olarak gelmektedirler. 

```lisp
(define (good-enough? guess x )
		(< (abs (- (square guess) x))
				0.001))
```

DAHİLİ TANIMLAR VE BLOK YAPISI 

```lisp
(define (sqrt x)
		(sqrt-iter 1.0 x))
		
(define (sqrt-iter guess x)
		(if (good-enough? guess x)
		guess
		(sqrt-iter (improve guess x) x)))
		
(define (good-enough? guess x)
		(< (abs (- (square guess) x)) 0.001))

(define (improve guess x)
		(average guess (/ x guess)))
```

Kullanıcı açısından önem arz eden prosedür square. Prosedürü alt prosedürlere ayırmak yerine tamamını tek bir sqrt prosedürü altında yazabilirdik, buna iç içe yapı (nested structure) ya da blok yapısı (block structure) denir.

```lisp
(define (sqrt x)
		(define (good-enough? guess x)
				(< (abs (- (square guess) x)) 0.001))
		(define (improve guess x) (average guess (/ x guess)))
		(define (sqrt-iter guess x)
				(if (good-enough? guess x)
						guess
						(sqrt-iter (improve guess x) x)))
		(sqrt-iter 1.0 x))
```

Bunu daha da basitleştirebiliriz. Tüm prosedürler sqrt’in alt prosedürü olduğuna göre x değişkeni de hepsi için geçerli dahili bir değişken. X’i dahili tanımlamada serbest değişken (free variable) olarak kullanabiliriz. Böylelikle her alt prosedürde bunu tekrardan tanımlamamıza gerek kalmaz. Bu duruma da *lexical scoping* denir. 

# Procedures and the Process They Generate

Şimdiye kadar satrançta taşların nasıl hareket ettiğini anladık diyebiliriz. Fakat henüz tipik açılışları, taktikleri ve stratejileri öğrenmedik. 

## Linear Recursion and Iteration

Faktöriyel formülüne bir bakalım

$$
n!=n⋅(n−1)⋅(n−2)⋯3⋅2⋅1.
$$

Ancak faktöriyeli hesaplamanın tek formülü bu değildir

$$
n!=n⋅[(n−1)⋅(n−2)⋯3⋅2⋅1]=n⋅(n−1)!.
$$

Yukarıdaki formülü bir prosedür olarak yazmaya çalışırsak da 

```lisp
(define (factorial n)
  (if (= n 1) 
      1 
      (* n (factorial (- n 1)))))
```

Prosedürün nasıl çalıştığını anlamak için substitution yani yerine koyma modelini uygulayabiliriz.

![Untitled](SICP%208dceb8ed39974f8fb1dc0bb5d6a7bc1c/Untitled%205.png)

Bundan farklı olarak öncelikle 1 ve 2’yi çarpıp sonrasında çıkan sonuç ile 3’ü çarpabilirdik. Sonrasında çıkan sonucu da 4 ile çarpardık ve istenilen sayıya kadar bu işlemi böyle devam ettirebilirdik. 

product← counter * product
counter← counter + 1

şeklinde her seferinde çarptığımız sayıyı birer artırarak da faktöriyeli hesaplayabilirdik. Bunu prosedür olarak yazmak istersek de

```lisp
(define (factorial n) 
  (fact-iter 1 1 n))

(define (fact-iter product counter max-count)
  (if (> counter max-count)
      product
      (fact-iter (* counter product)
                 (+ counter 1)
                 max-count)))
```

Bunun da nasıl çalıştığını anlamak için yerine koyma modelini kullanalım 

![Untitled](SICP%208dceb8ed39974f8fb1dc0bb5d6a7bc1c/Untitled%206.png)

Bu iki process aynı sonucu verseler de çalışma adımları biraz farklı. Görsel olarak baktığımızda da “şekil” olarak bir farkları var. İlk şekle bakıldığında onun bir okun ucuna benzediğini görebiliriz. Bu bir deferred operation (ertelenmiş operasyon). 
Yani öncelikle tüm parantezleri açıyor ve içerisindeki değerleri yerlerine koyuyor, bu işleme recursive (özyinelemeli) process denir. Burada çarpım işlemlerinin uzunluğu birçok bilginin aynı anda tutulmasını gerektirir ve tutulması gereken bu bilgiler lineer olarak artmaktadır. Bu sebepten dolayı da LINEAR RECURSIVE PROCESS olarak adlandırılır.

Diğer ikinci şeklimiz ise gittikçe uzayan bilgilere sahip değildir. İşlem için o anda gerekli olan değerlerle ilgilenir. Yine n’e bağlı olarak hesaplanacak adımlar lineer olarak uzadığı için bu süreç iterative process ya da LINEAR ITERATIVE PROCESS olarak adlandırılır. 

Bu iki süreç arasındaki fark:

Iteration yönteminde her adımda direktifler açıktır, hangi verilerle neler yapılacağı her satırda verilmiştir. Bu da sürecin herhangi bir yerinde kesilmesi durumunda kaldığı yerden devam edebileceği anlamına gelir. Fakat diğer yöntemde bu mümkün değildir, işlemler “saklanmış” bazı bilgiler ile işlem görür.

Önemli bir nokta ise recursive process ile recursive prosedürün birbirinden farklı şeyler olmasıdır. Recursive process sonucunda ilk fotoğrafta gördüğümüz gibi ok ucuna benzer bir yapı olmaktadır ve recursive prosedür bir yapı konusudur. Eğer ki prosedür içerisinde kendisini içeriyorsa bu yapı bakımından recursive bir prosedürdür. 

## Tree Recursion

Bir başka hesaplama kalıbı ise tree recursion, özyineleme ağacıdır. Fibonacci sayıları üzerinden örnek verecek olursak bu sayılar 0,1,1,2,3,5,8,13,21… şeklinde kendisinden önceki iki sayının toplamı şeklinde ilerleyen bir fonksiyondur   

![Untitled](SICP%208dceb8ed39974f8fb1dc0bb5d6a7bc1c/Untitled%207.png)

Bunu recursive prosedür olarak yazarsak da 

```
(define (fib n)
  (cond ((= n 0) 0)
        ((= n 1) 1)
        (else (+ (fib (- n 1))
                 (fib (- n 2))))))
```

![Untitled](SICP%208dceb8ed39974f8fb1dc0bb5d6a7bc1c/Untitled%208.png)

Fakat bu yöntem fibonacci sayılarını hesaplamak için berbat bir yöntemdir. Çünkü fib3 sağda ve solda 2 kez tekrar tekrar hesaplanıyor. Eğer ki sayımız 5 değil de daha büyük bir sayı olsaydı o zaman iş yükü çok daha gereksiz bir şekilde artacaktı. Bu yöntemdeki adım sayılar lineer değil üstel (exponentially) olarak artmaktadır.

Bunu linear iterative process olarak da hesaplayabilirdik. Bunu da a ve b gibi sayılarla tekrar eden işlemlerle yapabiliriz

a ← a +b 

b ← a

```lisp
(define (fib n)
  (fib-iter 1 0 n))

(define (fib-iter a b count)
  (if (= count 0)
      b
      (fib-iter (+ a b) a (- count 1))))
```

Fakat tüm bunler tree-recursive process’in işlevsiz olduğu anlamına gelmez. Hiyerarşik yapılı verilerde çok güçlü bir araç olarak kullanılır. 

Örnek: Bozukluk hesaplama

1 doları; yarım dolar, çeyreklik, dimes,nickels, pennies gibi farklı birimdeki paralarla nasıl elde edebiliriz? 

a miktar parayı n çeşit madeni para kullanarak hesaplamanın yolları eşittir:

- a miktarını ilk madeni para türü hariç tümünü kullanarak değiştirmenin yollarının sayısı +
- a-d miktarını tüm n çeşit madeni parayı kullanarak değiştirmenin yollarının sayısı, burada d birinci tür madeni paranın değeridir
- eğer ki a, 0 ise burada 1 yol vardır
- a 0’dan küçükse burada 0 yol vardır
- n 0 ise, burada 0 yol vardır

```lisp
(define (count-change amount)
  (cc amount 5))

(define (cc amount kinds-of-coins)
  (cond ((= amount 0) 1)
        ((or (< amount 0)
             (= kinds-of-coins 0))
         0)
        (else
         (+ (cc amount (- kinds-of-coins 1))
            (cc (- amount (first-denomination
                           kinds-of-coins))
                kinds-of-coins)))))

(define (first-denomination kinds-of-coins)
  (cond ((= kinds-of-coins 1) 1)
        ((= kinds-of-coins 2) 5)
        ((= kinds-of-coins 3) 10)
        ((= kinds-of-coins 4) 25)
        ((= kinds-of-coins 5) 50)))
        
(count-change 100)
;292
```

Bu yöntemde tree-recursive process kullanıldı 

## Orders of Growth

Prosedürlerin ne ne kadar kaynak kullandığını hesaplamamıza yarıyor. Giriş’in büyümesi ile işlem yükünün ne oranda arttığını hesaplıyor. 

Problemimizin boyutu n olsun, R(n)’de process’imizin n boyutundaki bir problemi çözmek için ne kadar kaynağa ihtiyaç duyduğu.  Bir sayının karekökünün hesaplanması gibi problemlerde n basamak sayısı olabilir. Matris çarpımında n matrisin bir satırı olabilir.  Bilgisayarlarda bu zamanla ölçülür. 

## Exponentiation

Üstel bir sayının hesaplaması argüman olarak b taban sayısını, pozitif bir üstel olan n’i alır ve b’nin n. üsteli olarak hesaplanır.

![image.png](SICP%208dceb8ed39974f8fb1dc0bb5d6a7bc1c/image.png)

 Bunu hesaplamak için recursive bir tanımlama kullanılabilir. 

```lisp
(define (expt b n)
	(if (= n 0)
		1
		(* b (expt b (- n 1))))
```

Bu lineer recursive bir process’tir. `Q(n)`adım için  `Q(n)`alana (space) gereklidir. Fibonacci sayısında yaptığımız gibi linear iteration olarak da yazabiliriz.

```lisp
(define (expt b n)
	(expt-iter b n 1))
	
(define (expt-iter b counter product)
	(if (= counter 0)
		product
			(expt-iter b
				(- counter 1)
				(* b product))))
```

Bu versiyonda `Q(n)`adım için `Q(1)`space gerekli.

Bu işlemi daha hızlı yapabilmek için kare hesaplama yöntemini de kullanabiliriz. Örneğin b’nin 8. kuvvetini hesaplamak istiyorsak 8’in yarısı olan 4’ü alırız ve b’nin 4. kuvvetini alıp sonrasında sonucun karesini alırız. 

![image.png](SICP%208dceb8ed39974f8fb1dc0bb5d6a7bc1c/image%201.png)

![image.png](SICP%208dceb8ed39974f8fb1dc0bb5d6a7bc1c/image%202.png)

```
(define (fast-expt b n)
  (cond ((= n 0)
         1)
        ((even? n)
         (square (fast-expt b (/ n 2))))
        (else
         (* b (fast-expt b (- n 1))))))
```

Bu logaritmik olarak büyüyen bir process. Büyüme oranı `O(log n)`. Aradaki farka bakacak olursak `O(n)`ile logaritmik process arasındaki fark n kattır. Logaritmik büyüyen process n kat daha hızlı çalışır. 

## Greatest Common Divisors (OBEB)

Ortak en büyük böleni hesaplamak için üç değişken tanımlayalım. a bölünen, b bölen, r ise kalan olsun. bölen sayıyı kalana bölerek formülümüzü oluşturabiliriz. 

GCD(a,b) = GCD(b,r)

> GCD(206, 40) = GCD(40,6)
                          = GCD(6,4)
                          = GCD(4,2)
                          = GCD(2,0) = 2
> 

```
(define (gcd a b)
  (if (= b 0)
      a
      (gcd b (remainder a b))))
```

Bu logaritmik olarak büyüyen bir iterative process

## Example: Testing for Primality

Bir sayının asal olup olmadığını nasıl bulabiliriz? 

Yollardan biri onu ikiden başlayarak sayılara bölmek ve bir sayının onu bölüp bölmediğine bakmak. Bu yöntemde sınırlayıcı olarak 2’den başlayıp asal olup olmadığını test edeceğimiz sayının yarısına kadar bölerek test edeceğiz. Çünkü bir sayının yarısından sonraki sayılar onu bölemez. Örneğin 100 sayısının asal olup olmadığını test edeceksek 50’den sonraki sayılara bakmamıza gerek yok çünkü bölemezler. 

```
(define (smallest-divisor n)
  (find-divisor n 2))

(define (find-divisor n test-divisor)
  (cond ((> (square test-divisor) n)
         n)
        ((divides? test-divisor n)
         test-divisor)
        (else (find-divisor
               n
               (+ test-divisor 1)))))

(define (divides? a b)
  (= (remainder b a) 0))
  
  (define (prime? n)
  (= n (smallest-divisor n)))
```

### Fermat Testi

`O(log n)`asal testi Fermat’nın Küçük Teorisine dayanır: "Eğer n bir asal sayı ise ve  a,  n'den küçük pozitif bir tam sayı ise, a sayısının n'inci kuvveti, a mod n'e denk olur.

mod nedir:  eğer iki sayı n ile bölündüğünde aynı kalana sahipse bu ikisi n modülünde denk olarak adlandırılır. Bir a sayısının n’e bölünmesi sonucunda kalana ise a mod n denir. 

Algoritma:

- Verilen n sayısı için, n’den küçük rastgele bir a sayısı seçin
- a üzeri n mod n’yi hesaplayın
    - Eğer sonuç a’ya eşit değilse, n kesinlikle asal değildir
    - Eğer sonuç a’ya eşitse, o zaman n’in asal olma olasılığı yüksektir.
        - Şimdi rastgele başka bir a sayısı seçin ve aynı yöntemle test edin
        - Eğer bu da denklemi sağlarsa asal olduğundan daha da emin olabiliriz. Daha fazla a sayısı denedikçe sonucun güvenilirliği de artar

```
(define (expmod base exp m)
  (cond ((= exp 0) 1)
        ((even? exp)
         (remainder
          (square (expmod base (/ exp 2) m))
          m))
        (else
         (remainder
          (* base (expmod base (- exp 1) m))
          m))))
          
 (define (fermat-test n)
  (define (try-it a)
    (= (expmod a n n) a))
  (try-it (+ 1 (random (- n 1)))))
  
  (define (fast-prime? n times)
  (cond ((= times 0) true)
        ((fermat-test n) 
         (fast-prime? n (- times 1)))
        (else false)))
```