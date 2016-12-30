Laravel 5 ve 4 için Mutlucell SMS
=========

[![Latest Stable Version](https://poser.pugx.org/ardakilic/mutlucell/v/stable.svg)](https://packagist.org/packages/ardakilic/mutlucell) [![Total Downloads](https://poser.pugx.org/ardakilic/mutlucell/downloads.svg)](https://packagist.org/packages/ardakilic/mutlucell) [![Latest Unstable Version](https://poser.pugx.org/ardakilic/mutlucell/v/unstable.svg)](https://packagist.org/packages/ardakilic/mutlucell) [![License](https://poser.pugx.org/ardakilic/mutlucell/license.svg)](https://packagist.org/packages/ardakilic/mutlucell)

Bu paket sayesinde Laravel 5.x veya 4.x kullanan projelerinizde [Mutlucell](http://www.mutlucell.com.tr/) altyapısını kullanarak tekli veya çoklu sms gönderebilir, bakiye ve originator ID sorgulayabilirsiniz. 

_Bu branch Laravel 5 içindir. Eğer bu paketi Laravel 4 üzerinde kullanmak istiyorsanız *1.x sürümünü*, `"ardakilic/mutlucell": "~1"` etiketi ile kullanmalısınız.

Uyarı, hata ve bilgilendirme için Türkçe ve de İngilizce dillerinde uyarı ve bilgi mesajlarını barındırır.


Kurulum (Laravel 5.x için)
-----------

* Öncelikle `composer.json` dosyanızdaki `require` kısmına aşağıdaki değeri ekleyin:

    ```json
    "ardakilic/mutlucell": "~2"
    ```

    Alternatif olarak `composer require ardakilic/mutlucell:~2` komutu ile de paketi ekleyebilirsiniz.
* Ardından eğer `composer.json dosyasını elinizle güncellediyseniz kodları projenize dahil etmek için Composer paketlerinizi güncellemelisiniz. `composer update` komutu ile bunu yapabilirsiniz.
* Şimdi de `app/config/app.php` dosyasını açın, `providers` dizisi içine en alta şunu girin:

    ```php
    Ardakilic\Mutlucell\MutlucellServiceProvider::class,
    ```
* Şimdi yine aynı dosyada `aliases` dizisi altına şu değeri girin:

    ```php
    'Mutlucell' => Ardakilic\Mutlucell\Facades\Mutlucell::class,
    ```
* Şimdi de environment'ınıza konfigürasyon dosyasını paylaşmalısınız. Bunun için aşağıdaki komutu çalıştırın:

    ```shell
    php artisan vendor:publish
    ```
* `config/mutlucell.php` dosyası paylaşılacak. Burada Mutlucell için size atanan kullanıcı adı, parola ve sender_id (originator) değerlerini, ve de diğer ayarları doldurmalısınız.

**Laravel 4.x sürümünde kullanım bilgisi için [ilgili branch'ın README.md dosyasına](https://github.com/Ardakilic/laravel-mutlucell-sms/tree/l4) bakmalısınız.**

Kullanım
-------------

####Birine o anda tekil SMS göndermek için:

```php
$send = Mutlucell::send('05312345678', 'Merhaba');
var_dump(Mutlucell::parseOutput($send));
```

####SMS gönderildi mi ?

```php
$send = Mutlucell::send('05312345678', 'Merhaba');
if(Mutlucell::getStatus($send)) {
    echo 'SMS başarı ile gönderildi!';
} else {
    echo 'SMS gönderilemedi';
}
```

####Birden fazla kişiye aynı anda aynı SMS'i göndermek için:

```php
$kisiler = ['00905312345678', '+905351114478', '05369998874', '5315558896'];
$send = Mutlucell::sendBulk($kisiler, 'Merhaba');
var_dump(Mutlucell::parseOutput($send));
```

Veya 

```php
$send = Mutlucell::sendBulk('00905312345678, +905351114478, 05369998874, 5315558896', 'Merhaba');
Mutlucell::parseOutput($send);
```

####Birden fazla kişiye aynı anda farklı SMS'ler göndermek için:

```php
$kisiMesajlar = [
    ['05315558964', 'Merhaba1'],
    ['+905415589632', 'Merhaba2'],
    ['00905369998874', 'Merhaba3']
];
$send = Mutlucell::sendMulti($kisiMesajlar);
var_dump(Mutlucell::parseOutput($send));
```

Veya

```php
$kisiMesajlar = [
    ['05315558964' => 'Merhaba1'],
    ['+905415589632' => 'Merhaba2'],
    ['00905369998874' => 'Merhaba3']
];
$send = Mutlucell::sendMulti2($kisiMesajlar);
var_dump(Mutlucell::parseOutput($send));
```

####Kalan Kontör Sorgulaması için:

```php
var_dump(Mutlucell::checkBalance());
```

####Originatörleri listelemek için:

```php
var_dump(Mutlucell::listOriginators());
```

#### Gelecek bir tarihe SMS yollamak için:

```php
Mutlucell::send('05312223665', 'Geç gidecek mesaj', '2099-06-30 15:00'); //saniye yok, dikkat!
```

#### Farklı bir Originatör (Sender ID) kullanarak SMS yollamak için:

```php
Mutlucell::send('05312223665', 'merhaba', '', 'diğerOriginator');
```

Yapılacaklar
----
* ?

Notlar
----
* 29 Aralık 216'dan önce kurulum gerçekleştirdiyseniz config dosyanıza 2 değer eklemeniz lazım:

```php
// SMS Charset
'charset' => 'default', // Values are: default, turkish, unicode

//Append Unsubscribe text and link for receivers
'append_unsubscribe_link' => false,
```

Bu 2 değer SMS gönderim karakter dilini ve de sms'lerin sonuna gelecek olan "sms aboneliğinden çık" linkini barındırmakta.

* 22 Temmuz 2014'den önce kurulum gerçekleştirdiyseniz config dosyasını ortama yeniden paylaşmalısınız

Lisans
----

Mu yazılım paketi MIT lisansı ile lisanslanmıştır.
