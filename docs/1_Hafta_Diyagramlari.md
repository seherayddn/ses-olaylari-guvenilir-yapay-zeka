usecaseDiagram
    actor Kullanıcı
    actor "Yetkili Araştırmacı" as Araştırmacı

    Kullanıcı --> (Ses Dosyası Yükleme)
    Kullanıcı --> (Analiz Sürecini Başlatma)
    Kullanıcı --> (Zaman Çizelgesini İnceleme)
    Kullanıcı --> (Model İddialarını ve Kanıtları İnceleme)
    
    Araştırmacı --> (Deney Sonuçlarını Listeleme)
    Araştırmacı --> (Model Karşılaştırmalarını İnceleme)

    (Analiz Sürecini Başlatma) ..> (Dosya Format Kontrolü) : <<include>>
    (Model İddialarını ve Kanıtları İnceleme) ..> (Belirsizlik Durumunda Uyarı Verme) : <<extend>>


    graph TD
    subgraph İstemci_Arayüz
        UI[React Frontend / Figma Mockup Birebir]
    end

    subgraph Sunucu_Backend
        API[FastAPI Backend]
    end

    subgraph Yapay_Zeka_Katmanı
        AudioProc[Ses Ön İşleme - librosa]
        BaseModel[Temel Ses-Dil Modeli]
        SED[Ses Olay Tespiti]
        ECHO[ECHO Kanıt Değerlendirici]
    end

    subgraph Veritabanı
        DB[(SQL Server)]
    end

    UI <--> API
    API --> AudioProc
    AudioProc --> BaseModel
    AudioProc --> SED
    BaseModel --> ECHO
    SED --> ECHO
    ECHO --> API
    API <--> DB



graph TD
    A[Ses Yüklendi] --> B{Boyut Uygun mu?}
    B -- Hayır --> C[Hata: Dosya Çok Büyük]
    B -- Evet --> D{Format WAV/MP3 mü?}
    D -- Hayır --> E[Hata: Desteklenmeyen Format]
    D -- Evet --> F[librosa ile Oku]
    F --> G{Dosya Bozuk mu?}
    G -- Evet --> H[Hata: Okunamadı]
    G -- Hayır --> I[Analize Hazır]


    
  graph TD
    A[Model Açıklama Üretti] --> B[Açıklamayı İddialara Böl]
    B --> C[Olay Zaman Damgalarını Al]
    C --> D[İddia ve Kanıtı Eşleştir]
    D --> E{Kanıt Destekliyor mu?}
    E -- Evet --> F[Sonuç: Destekleniyor]
    E -- Hayır, Zıt --> G[Sonuç: Çelişiyor]
    E -- Kanıt Yok --> H[Sonuç: Doğrulanamıyor]



    graph TD
    A[İddia Durumu: Doğrulanamıyor] --> B[Belirsizlik Puanı Hesapla]
    B --> C{Puan Eşiği Aştı mı?}
    C -- Hayır --> D[Kullanıcıya Temkinli Göster]
    C -- Evet --> E[Modeli Sustur / Cevap Verme]
    E --> F[Uyarı: Yeterli Kanıt Bulunamadı]


graph LR
    HamSes[Ham Ses] --> M1[Ses-Dil Modeli]
    HamSes --> M2[Ses Olay Tespiti]
    M1 --> İddialar[Metin İddiaları]
    M2 --> Zaman[Zaman Damgaları]
    İddialar --> KanitMotoru[Kanıt Motoru]
    Zaman --> KanitMotoru
    KanitMotoru --> Cikti1[Doğrulanmış Sonuç]
    KanitMotoru --> Cikti2[Belirsizlik Kararı]



    
    
