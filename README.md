# Laravel Telegram logger

Kirim log ke obrolan Telegram melalui bot Telegram

## Install

```

komposer memerlukan grkamil/laravel-telegram-logging

```

Tentukan Token Bot Telegram dan id obrolan (id telegram pengguna) dan tetapkan sebagai parameter lingkungan.
Tambahkan ke <b>.env</b>

```
TELEGRAM_LOGGER_BOT_TOKEN=id:token
TELEGRAM_LOGGER_CHAT_ID=chat_id
```


Tambahkan ke file <b>config/logging.php</b> saluran baru:

```php
'telegram' => [
     'driver' => 'khusus',
     'melalui' => Pencatat\TelegramLogger::kelas,
     'tingkat' => 'debug',
]
```

Jika saluran log default Anda adalah tumpukan, Anda dapat menambahkannya ke saluran <b>tumpukan</b> seperti ini
```php
'tumpukan' => [
     'driver' => 'tumpukan',
     'saluran' => ['tunggal', 'telegram'],
]
```

Atau Anda cukup mengubah saluran log default di .env
```
LOG_CHANNEL=telegram
```

Publikasikan file konfigurasi dan tampilan
```
vendor artisan php: publikasikan --provider "Logger\TelegramLoggerServiceProvider"
```

## Format Pencatatan Telegram

Anda dapat memilih di antara dua format berbeda yang dapat Anda tentukan dalam file `.env` seperti ini :

```
# Gunakan templat log minimal
TELEGRAM_LOGGER_TEMPLATE = laravel-telegram-logging::minimal

# Atau gunakan yang kompatibel mundur (pengaturan default digunakan bahkan tanpa memasukkan baris ini)
TELEGRAM_LOGGER_TEMPLATE = laravel-telegram-logging::standard
```

Dimungkinkan untuk membuat templat blade lain dan mereferensikannya di entri `TELEGRAM_LOGGER_TEMPLATE`

## Buat bot

Untuk menggunakan paket ini, Anda perlu membuat bot Telegram

1. Kunjungi @BotFather di Telegram
2. Kirim ``/newbot``
3. Siapkan nama dan nama bot untuk bot Anda.
4. Dapatkan token dan tambahkan ke file .env Anda (tertulis di atas)
5. Buka bot Anda dan kirim pesan ``/mulai``

## Ubah template log saat runtime

1. Ubah konfigurasi untuk template.
```php
config(['telegram-logger.template'=>'laravel-telegram-logging::custom'])
```
2. Gunakan `Log` seperti biasa.

## Mengonfigurasi id atau token obrolan yang berbeda per saluran

1. Tambahkan `chat_id` atau `token` ke saluran di `config/logging.php`. Menimpa `config('telegram.chat_id')`.
```php
[
     'saluran' => [
         [
             'perusahaan' => [
                 'driver' => 'khusus',
                 'melalui' => TelegramLogger::kelas,
                 'chat_id' => env('TELEGRAM_COMPANY_CHAT_ID'),
                 'token' => env('TELEGRAM_COMPANY_BOT_TOKEN'),
                 'tingkat' => 'debug'
             ],

             'operasi' => [
                 'driver' => 'khusus',
                 'melalui' => TelegramLogger::kelas,
                 'chat_id' => env('TELEGRAM_OPERATIONS_CHAT_ID'),
                 'token' => env('TELEGRAM_OPERATIONS_BOT_TOKEN'),
                 'tingkat' => 'debug'
             ]
         ]
     ]
]
```

2. Gunakan `Log` seperti biasa.
## Dukungan Lumen

Agar berfungsi dengan Lumen, Anda juga perlu menjalankan dua langkah:

1. Tempatkan file config/telegram-logger.php dengan kode berikut:
```php
<?php

kembali [
     // Token bot pencatat telegram
     'token' => env('TELEGRAM_LOGGER_BOT_TOKEN'),

     // Id obrolan telegram
     'chat_id' => env('TELEGRAM_LOGGER_CHAT_ID'),
    
     // Anda dapat menentukan template khusus untuk pesan
     // misalnya: logging.template
     // 'templat' => 'beberapa jalur tampilan Anda'
];
```

2. Batalkan komentar ```$app->withFacades();``` dan konfigurasikan file ```$app->configure('telegram-logger');``` di bootstrap/app.php
3. Tempatkan file logging Laravel/Lumen default ke config/logging.php (untuk menambahkan saluran baru).

## Dukungan proxy
Untuk menggunakan server proxy, setel variabel di .env
```
TELEGRAM_LOGGER_PROXY=proxy_server.com:port
```
