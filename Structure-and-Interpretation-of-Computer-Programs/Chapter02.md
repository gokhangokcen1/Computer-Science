# Bölüm 2
## Bizi ne bekliyor? 
İlk bölümde temel veri tiplerinden biri olan sayılarla ve temel aritmetik işlemlerle ilgilenmiştik. Bu bölümde daha bileşik verilerle (compound data) ilgileneceğiz. 

Lineer kombinasyon hesabı (ax+by) yapan bir prosedür yazalım

 

```lisp
(define (linear-combination a b x y)
  (+ (* a x) (* b y)))
```

Fakat burada yalnızca sayılar ile ilgilenmeyeceğiz. Temel aritmetik işlemlerin diğer veri tipleri ile doğru çalışmasını istiyoruz bu durumda yapacağımız şey işlemlerimizin bir soyutlamasını yaratmak olacaktır. 

```lisp
(define (linear-combination a b x y)
  (add (mul a x) (mul b y)))
```

Buradaki add (toplama), mul (çarpma) farkli prosedürlerdir. Amacımız daha önce de verdiğimiz kumanda örneği ile aynı. Kullanıcının detayları bilmesi, prosedürün nasıl çalıştığını bilmesi önemli değil. Kumandanın kendi işlevlerini yapıp yapmadığıdır. Bu da bize bir kolaylık sağlıyor. Bu şekilde ifade etmek çok daha basit gözüküyor ve basit gözükmekten çok daha önemlisi kendi yazacağımız bu prosedürlerde farklı veri tiplerinin kullanılması durumunda nasıl yollar izleneceğini de kullanıcının kafasını karıştırmadan belirleyebileceğiz. 

> Karmaşık verilerle başa çıkmada önemli bir fikir, kullandığımız prosedürlerin sadece ilkel veri nesnelerini değil, aynı zamanda karmaşık veri nesnelerini de birleştirmemize izin vermesi gerektiğidir.
>
