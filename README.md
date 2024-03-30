**A comprehensive collection of visual components for WordPress**

Langkah demi langkah penjelasan dari script PHP [shortcodes-ultimate.php](shortcodes-ultimate/shortcodes-ultimate.php) yang diberikan adalah sebagai berikut:

1. **Komentar Header**: Bagian ini berisi informasi tentang plugin, seperti nama plugin, URI plugin, penulis, URI penulis, deskripsi plugin, domain teks, lisensi, versi plugin, dan informasi lainnya yang terkait dengan plugin.

2. **Pengecekan ABSPATH**: Script memeriksa apakah konstanta ABSPATH telah didefinisikan. ABSPATH adalah konstanta yang didefinisikan oleh WordPress untuk menunjukkan jalur lengkap ke direktori WordPress. Jika konstanta ABSPATH tidak didefinisikan, maka script akan menghentikan eksekusi dengan pernyataan `exit;`. Ini memastikan bahwa script tidak dapat diakses secara langsung dan hanya dapat diakses melalui instalasi WordPress.

3. **Pengecekan Fungsi su_fs()**: Script memeriksa apakah fungsi `su_fs()` sudah ada. Jika fungsi ini sudah ada, maka akan dipanggil metode `set_basename()` pada objek yang dikembalikan oleh fungsi `su_fs()`. Metode ini mengatur nilai `basename` ke false dan menggunakan `__FILE__` sebagai parameter kedua. Jika fungsi `su_fs()` belum ada, maka akan dilakukan definisi fungsi `su_fs()`.

4. **Definisi Fungsi su_fs()**: Fungsi `su_fs()` dibuat sebagai fungsi bantuan untuk mengakses SDK Freemius dengan mudah. Fungsi ini menggunakan variabel global `$su_fs` untuk menyimpan instansi SDK Freemius. Jika variabel `$su_fs` belum diinisialisasi, maka SDK Freemius akan diinisialisasi menggunakan fungsi `fs_dynamic_init()` dengan parameter yang diberikan.

5. **Inisialisasi Freemius**: SDK Freemius diinisialisasi dengan parameter yang diberikan, termasuk ID, slug, kunci publik, dan pengaturan lainnya yang diperlukan. Setelah inisialisasi selesai, fungsi `su_fs()` akan mengembalikan instansi SDK Freemius yang telah diinisialisasi.

6. **Sinyal bahwa SDK telah diinisialisasi**: Setelah SDK Freemius diinisialisasi, script memanggil fungsi `do_action('su_fs_loaded')` untuk memberi tahu bahwa SDK telah diinisialisasi.

7. **Definisi Konstanta SU_PLUGIN_FILE dan SU_PLUGIN_VERSION**: Konstanta `SU_PLUGIN_FILE` didefinisikan dengan menggunakan `__FILE__`, yang merupakan jalur lengkap ke file skrip saat ini. Konstanta `SU_PLUGIN_VERSION` juga didefinisikan dengan nilai '7.0.5'.

8. **Memasukkan File Plugin.php**: Akhirnya, script memasukkan file `plugin.php` yang berisi logika utama dari plugin Shortcodes Ultimate.

Ini adalah penjelasan langkah demi langkah dari script PHP yang diberikan.

<hr>

```
<?php

// 1. **Komentar Header**:

/**
 * Nama Plugin: Shortcodes Ultimate
 * URI Plugin: https://getshortcodes.com/
 * Penulis: Vova Anokhin
 * URI Penulis: https://getshortcodes.com/
 * Deskripsi: Sebuah koleksi komponen visual yang komprehensif untuk WordPress
 * Text Domain: shortcodes-ultimate
 * Lisensi: GPLv3
 * Versi: 7.0.3
 * Membutuhkan PHP: 5.4
 * Membutuhkan minimal: 5.0
 * Diuji hingga: 6.4
 */


// 2. **Pengecekan ABSPATH **:  Memastikan script tidak dapat diakses secara langsung

if (!defined('ABSPATH')) {
    exit;
}


// 3. **Pengecekan Fungsi su_fs()**: Memeriksa apakah fungsi su_fs() sudah ada
if (function_exists('su_fs')) {
    //  Menetapkan basename ke false jika su_fs() sudah ada
    su_fs()->set_basename(false, __FILE__);
} else {
    //  4. **Definisi Fungsi su_fs()**: Membuat fungsi bantuan su_fs() jika belum ada
    if (!function_exists('su_fs')) {
        // 5. **Inisialisasi Freemius**: Fungsi bantuan untuk mengakses SDK dengan mudah

        function su_fs()
        {
            global $su_fs;

            if (!isset($su_fs)) {
                // Memasukkan SDK Freemius
                require_once dirname(__FILE__) . '/freemius/start.php';
                $su_fs = fs_dynamic_init(array(
                    'id'                => '7180',
                    'slug'              => 'shortcodes-ultimate',
                    'premium_slug'      => 'shortcodes-ultimate-pro',
                    'type'              => 'plugin',
                    'public_key'        => 'pk_c9ecad02df10f17e67880ac6bd8fc',
                    'is_premium'        => false,
                    'premium_suffix'    => 'Pro',
                    'has_addons'        => false,
                    'has_paid_plans'    => true,
                    'menu'              => array(
                        'slug'       => 'shortcodes-ultimate',
                        'first-path' => 'admin.php?page=shortcodes-ultimate',
                        'contact'    => false,
                        'support'    => false,
                    ),
                    'opt_in_moderation' => array(
                        'new'       => 100,
                        'updates'   => 0,
                        'localhost' => true,
                    ),
                    'is_live'           => true,
                ));
            }

            return $su_fs;
        }


        // 5. **Inisialisasi Freemius**: Memanggil fungsi su_fs() untuk menginisialisasi Freemius
        su_fs();
        // 6. **Sinyal bahwa SDK telah diinisialisasi**: Memberi sinyal bahwa SDK telah diinisialisasi

        do_action('su_fs_loaded');
    }
}


// 7. **Definisi Konstanta SU_PLUGIN_FILE dan SU_PLUGIN_VERSION**: Mendefinisikan konstanta SU_PLUGIN_FILE dan SU_PLUGIN_VERSION jika su_fs() belum ada
define('SU_PLUGIN_FILE', __FILE__);
define('SU_PLUGIN_VERSION', '7.0.3');

// 8. **Memasukkan File Plugin.php**: Memasukkan file plugin.php jika su_fs() belum ada  

require_once dirname(__FILE__) . '/plugin.php';

```
<hr>

Langkah demi langkah dari script PHP [plugin.php](shortcodes-ultimate/plugin.php) yang diberikan adalah sebagai berikut:

1. **Pemanggilan `require_once`**: Script ini memanggil fungsi `require_once` untuk memuat dua file PHP yang diperlukan:
   - `class-shortcodes-ultimate-activator.php`: File ini berisi kelas `Shortcodes_Ultimate_Activator` yang bertanggung jawab untuk menangani aktivasi plugin.
   - `class-shortcodes-ultimate.php`: File ini berisi definisi kelas `Shortcodes_Ultimate` yang merupakan inti dari plugin Shortcodes Ultimate.

2. **Pendaftaran `register_activation_hook`**: Script ini mendaftarkan fungsi `Shortcodes_Ultimate_Activator::activate` sebagai hook aktivasi plugin. Fungsi ini akan dieksekusi ketika plugin diaktifkan.

3. **Panggilan `call_user_func`**: Script ini menggunakan fungsi `call_user_func` untuk membuat pemanggilan anonim fungsi. Di dalamnya, terdapat kondisi untuk memastikan bahwa plugin tidak berjalan selama proses aktivasi. Jika aksi 'plugins_loaded' sudah dilakukan, artinya plugin sudah terinisialisasi, dan proses berhenti. Jika belum, maka objek `Shortcodes_Ultimate` akan dibuat dengan parameter yang diberikan: `SU_PLUGIN_FILE`, `SU_PLUGIN_VERSION`, dan awalan string 'shortcodes-ultimate-'.

4. **Inisialisasi Plugin**: Objek `Shortcodes_Ultimate` dibuat dengan menggunakan kelas yang telah didefinisikan sebelumnya. Objek ini diinisialisasi dengan parameter `SU_PLUGIN_FILE`, `SU_PLUGIN_VERSION`, dan awalan string 'shortcodes-ultimate-'.

5. **Pemanggilan Hook 'su/ready'**: Setelah objek plugin dibuat, script memanggil hook 'su/ready' dengan menyertakan objek plugin sebagai argumen. Ini memungkinkan hook tersebut untuk mengeksekusi aksi tertentu setelah plugin siap digunakan.

Ini adalah penjelasan langkah demi langkah dari script PHP [plugin.php](shortcodes-ultimate/plugin.php) yang diberikan :

```
<?php
// 1. **Pemanggilan `require_once`**:
require_once dirname( __FILE__ ) . '/includes/class-shortcodes-ultimate-activator.php';
require_once dirname( __FILE__ ) . '/includes/class-shortcodes-ultimate.php';
// 2. **Pendaftaran `register_activation_hook`**:
register_activation_hook( SU_PLUGIN_FILE, array( 'Shortcodes_Ultimate_Activator', 'activate' ) );
3. **Panggilan `call_user_func`**: 
call_user_func( function () {
    // don't run the plugin during activation (after plugins_loaded)
    if ( did_action( 'plugins_loaded' ) ) {
        return;
    }
// 4. **Inisialisasi Plugin**: 
    $plugin = new Shortcodes_Ultimate( SU_PLUGIN_FILE, SU_PLUGIN_VERSION, 'shortcodes-ultimate-' );
// 5. **Pemanggilan Hook 'su/ready'**: 
    do_action( 'su/ready', $plugin );
} );

```
