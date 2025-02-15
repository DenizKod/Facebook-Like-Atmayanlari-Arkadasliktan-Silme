# Facebook Like Atmayanlari Arkadadasliktan Silme

### ADIM 1 (BETİK 1)

<p>- Facebook dilini Türkçe yap</p>
<p>- Tampermonkey eklentisi tarayıcınıza ekleyin</p>
<p>- Yeni Betik oluştura tıklayın</p>

![image](https://github.com/DenizKod/ARK-ISTEGI-IPTAL-ETME/assets/168285638/7e1b2696-803e-447a-ae3f-f7844a44d28f)

### Aşağıdaki kodu kopyala ve "yeni betik" sayfasındaki tüm eski kodları baştan sona sil ve alttaki kodu eksiksiz ekle ve betiği kaydet
```
// ==UserScript==
// @name         Facebook Beğeni Listesi Kopyalayıcı - F4 ile Kopyala, Virgülle Ayır
// @namespace    http://tampermonkey.net/
// @version      0.3
// @description  Facebook postlarında F4 tuşu ile beğeni yapan kullanıcıların isimlerini virgülle ayırarak kopyala
// @author       You
// @match        www.facebook.com/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    // Klavye olayını dinle
    document.addEventListener('keydown', function(e) {
        // F4 tuşuna basıldığında işlemi gerçekleştir
        if (e.key === 'F4') {
            e.preventDefault();  // F4'ün varsayılan işlevini engelle

            // Beğeni yapanların listesini seç
            var likeList = document.querySelectorAll('div[data-visualcompletion="ignore-dynamic"] a[role="link"]');
            var names = [];
            likeList.forEach(function(user) {
                // İsimleri al ve boşluklardan temizle
                var name = user.getAttribute("aria-label");
                if (name && name !== "Herkesi görmek için bağlantı") {  // spesifik metni hariç tut
                    // İstenmeyen metinleri temizle
                    name = name.replace("'in profil resmi", "").trim();
                    name = name.replace("Eki görmek için tıkla", "").trim();
                    name = name.replace("'in hikayesi", "").trim();
                    name = name.replace("Hikayeni gör", "").trim();
                    name = name.replace("Hikaye Oluştur", "").trim();
                    if (name) {
                        names.push(name);
                    }
                }
            });

            // İsimleri virgülle ayırarak panoya kopyala
            var formattedNames = names.join(', ');
            navigator.clipboard.writeText(formattedNames).then(function() {
                alert("Başarıyla kopyalandı: " + names.length + " kullanıcı.");
            }, function() {
                alert("Kopyalama başarısız!");
            });
        }
    });
})();
```
## NASIL ÇALIŞIR?

<p>- herhangi bir post'un içine gir ve like atanlar kısmını aç</p>
<p>- En aşağı kadar kaydır</p>
<p>- En aşağı indiğinde like kutusuna kapanmayacak şekilde 1 kere tıkla</p>
<p>- F4 tuşuna 1 kere bas</p>
<p>- Kopyalanan isimleri bir metin belgesine kaydet, birazdan lazım olacak</p>

# ADIM 2 (BETİK 2)

<p>- Kopyaladığın isimleri şimdi bu kodun içerisine ekleyeceğiz.</p>
<p>- Tekrar 2. bir yeni betik oluşturun</p>

### 2. kodu da yeni oluşturduğunuz betik sayfasına yapıştırın ama kaydetmeyin. çünkü kodun altında ne yapmanız gerektiğini anlatacağım.

```
// ==UserScript==
// @name         Facebook Profil Kartı Kaldırma
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  Facebook'ta belirtilen isimlere sahip kullanıcıların profil kartlarını DOM'dan kaldırır.
// @author       Deniz KOD
// @match        www.facebook.com/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    // Tüm isimler tek bir string içinde, virgülle ayrılmış şekilde
    const gizlenecekKullaniciAdlariString = "İsim 1, İsim 2, İsim 3, İsim4";
    // String'i virgülle ayırarak diziye dönüştür
    const gizlenecekKullaniciAdlari = gizlenecekKullaniciAdlariString.split(", ");

    // F6 tuşuna basıldığında silme işlemi başlat
    document.addEventListener('keydown', function(e) {
        if (e.key === 'F6') {
            const profilKartlari = document.querySelectorAll('div[class*="x1lq5wgf"]:not([data-visualcompletion="ignore-dynamic"])');
            profilKartlari.forEach(kart => {
                const isimElementi = kart.querySelector('span[dir="auto"]');
                if (isimElementi && gizlenecekKullaniciAdlari.includes(isimElementi.innerText.trim())) {
                    kart.remove(); // Elementi DOM'dan tamamen kaldır
                }
            });
        }
    });
})();
```

<p> BETİK 2'DE bu kısmı bulun : const gizlenecekKullaniciAdlariString = "İsim 1, İsim 2, İsim 3, İsim4"; 
az önce kopyaladığınız metin belgesindeki isimleri çift tırnağın arasına ekleyin</p>

![image](https://github.com/DenizKod/FacebookBGY/assets/168285638/d2049898-5da0-485f-8512-155b9cda6467)

# ADIM 3 (BETİK 3)

<p>- LİKE atan herkesin listede kaldığına emin olduysanız aşağıdaki 3. betiği de eklemeye hazırsınız</p>

**ARKADAŞLARI OTOMATİK SİLME BETİĞİ (BETİK 3)**

```
// ==UserScript==
// @name         Arkadaş Listesinden Otomatik Silme
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  BGY dışı herkesi silme
// @author       Deniz KOD
// @match        www.facebook.com/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    document.addEventListener('keydown', function(e) {
        if (e.key === 'F2') {
            console.log("Silme işlemi başlatıldı.");
            startRemovingFriends();
        } else if (e.key === 'F4') {
            console.log("Silme işlemi durduruldu.");
            stopRemovingFriends();
        }
    });

    let isRunning = false;
    let triedToRemove = new Set();  // Zaten silinmeye çalışılmış arkadaşları takip etmek için

    function startRemovingFriends() {
        isRunning = true;
        removeNextFriend();
    }

    function stopRemovingFriends() {
        isRunning = false;
    }

    function removeNextFriend() {
        if (!isRunning) return;

        const allFriendButtons = Array.from(document.querySelectorAll('div[aria-label="Arkadaşlar"]'));
        const friendButton = allFriendButtons.find(button => !triedToRemove.has(button));

        if (friendButton) {
            triedToRemove.add(friendButton);  // Bu arkadaşı işaretleyerek gelecekte tekrar denemeyi önle
            friendButton.click();

            setTimeout(() => {
                const allMenuItems = Array.from(document.querySelectorAll('[role="menuitem"]'));
                const removeOption = allMenuItems.find(item => item.textContent.includes("Arkadaşlarımdan Çıkar"));

                if (removeOption) {
                    removeOption.click();
                    console.log("Arkadaşlarımdan Çıkar seçeneği tıklandı.");
                    setTimeout(confirmDeletion, 500);
                } else {
                    console.log("Arkadaşlarımdan Çıkar seçeneği bulunamadı.");
                    handlePopupError();
                }
            }, 1000);
        } else {
            console.log("Daha fazla arkadaş butonu bulunamadı.");
        }
    }

    function confirmDeletion() {
        const confirmButton = document.querySelector('div[aria-label="Onayla"]');
        if (confirmButton) {
            confirmButton.click();
            console.log("Onayla butonuna basıldı.");
            setTimeout(removeNextFriend, 1500);
        } else {
            console.log("Onayla butonu bulunamadı.");
            handlePopupError();
        }
    }

    function handlePopupError() {
        const errorButton = document.querySelector('span[class*="Tamam"]');
        if (errorButton) {
            errorButton.click();
            console.log("Hata mesajı 'Tamam' butonu tıklandı, devam ediliyor.");
        }
        setTimeout(removeNextFriend, 10);
    }
})();
```
<p>- Bu koduda 3. betik olarak Tampermonkey'e ekledikten sonra profilinize gidin ve arkadaşların olduğu kısma girin</p>
<p>- Listenin en en en en en altına indikten sonra F6 tuşuna basın. böylelikle az önce isimlerini kopyaladığınız herkes arkadaş listesinden kaybolacak (sayfa yenilene kadar kaybolurlar)</p>
<p>- Herkesin kaybolduğuna emin olduysanız F2 tuşuna basarak silme işlemine başlayın.</p>




### DİKKAT : Bazı kişilerin profili "facebook tarafından banlandığı için o kişileri silemiyorsunuz"

![image](https://github.com/DenizKod/FacebookBGY/assets/168285638/baa299f3-c8f7-4f7e-bed4-58d829308df4)

Bot bu durumda takılma yaşıyor ve sürekli aynı kişiyi silmeye çalışıyor. düzeltmek için "TAMAM" butonuna bastıktan sonra hemen yanındaki arkadaşınıza tıklayın. aşağıdaki görselde işaretli yere tıklamanız gerekiyor.

![image](https://github.com/DenizKod/FacebookBGY/assets/168285638/9cb56c73-28b0-4eb6-922a-9a11a9614f95)

### NOT : İhlal yememek için kodun içerisine bekleme süresi ekledim. insana yakın hızda siliyor.
