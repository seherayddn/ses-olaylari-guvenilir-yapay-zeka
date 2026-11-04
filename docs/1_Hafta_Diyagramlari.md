### 1. Kullanım Senaryosu (Use Case) Diyagramı
```mermaid
flowchart LR
    User(( Kullanıcı ))
    Admin(( Yetkili Araştırmacı ))

    UC1([ Ses Dosyası <br> Yükleme ])
    UC2([ Analiz Sürecini <br> Başlatma ])
    UC3([ Zaman Çizelgesini <br> İnceleme ])
    UC4([ Model İddialarını ve <br> Kanıtları İnceleme ])
    UC5([ Deney Sonuçlarını <br> Listeleme ])
    UC6([ Model Karşılaştırmalarını <br> İnceleme ])

    User --- UC1
    User --- UC2
    User --- UC3
    User --- UC4
    
    Admin --- UC5
    Admin --- UC6
```

### 2. Sistem Mimarisi Diyagramı
```mermaid
graph TD
    subgraph İstemci_Arayüz
        UI[ React Frontend / <br> Figma Mockup Birebir ]
    end

    subgraph Sunucu_Backend
        API[ FastAPI Backend ]
    end

    subgraph Yapay_Zeka_Katmanı
        AudioProc[ Ses Ön İşleme ]
        BaseModel[ Temel Ses-Dil <br> Modeli ]
        SED[ Ses Olay <br> Tespiti ]
        ECHO[ ECHO Kanıt <br> Değerlendirici ]
    end

    subgraph Veritabanı
        DB[( SQL Server )]
    end

    UI <--> API
    API --> AudioProc
    AudioProc --> BaseModel
    AudioProc --> SED
    BaseModel --> ECHO
    SED --> ECHO
    ECHO --> API
    API <--> DB
```

### 3. Akış Diyagramları

**Akış 1: Ses Yükleme**
```mermaid
graph TD
    A[ Ses Yüklendi ] --> B{ Boyut <br> Uygun mu? }
    B -- Hayır --> C[ Hata: Dosya <br> Çok Büyük ]
    B -- Evet --> D{ Format <br> WAV/MP3 mü? }
    D -- Hayır --> E[ Hata: Desteklenmeyen <br> Format ]
    D -- Evet --> F[ librosa <br> ile Oku ]
    F --> G{ Dosya <br> Bozuk mu? }
    G -- Evet --> H[ Hata: Okunamadı ]
    G -- Hayır --> I[ Analize Hazır ]
```

**Akış 2: ECHO Kanıt Eşleştirme**
```mermaid
graph TD
    A[ Model Açıklama <br> Üretti ] --> B[ Açıklamayı <br> İddialara Böl ]
    B --> C[ Olay Zaman <br> Damgalarını Al ]
    C --> D[ İddia ve Kanıtı <br> Eşleştir ]
    D --> E{ Kanıt <br> Destekliyor mu? }
    E -- Evet --> F[ Sonuç: <br> Destekleniyor ]
    E -- Hayır --> G[ Sonuç: <br> Çelişiyor ]
    E -- Kanıt Yok --> H[ Sonuç: <br> Doğrulanamıyor ]
```

**Akış 3: Belirsizlik Kararı**
```mermaid
graph TD
    A[ İddia Durumu: <br> Doğrulanamıyor ] --> B[ Belirsizlik Puanı <br> Hesapla ]
    B --> C{ Puan Eşiği <br> Aştı mı? }
    C -- Hayır --> D[ Kullanıcıya <br> Temkinli Göster ]
    C -- Evet --> E[ Modeli Sustur / <br> Cevap Verme ]
    E --> F[ Uyarı: Yeterli <br> Kanıt Bulunamadı ]
```

### 4. AI Pipeline
```mermaid
graph LR
    HamSes[ Ham Ses ] --> M1[ Ses-Dil Modeli ]
    HamSes --> M2[ Ses Olay Tespiti ]
    M1 --> İddialar[ Metin İddiaları ]
    M2 --> Zaman[ Zaman Damgaları ]
    İddialar --> KanitMotoru[ Kanıt Motoru ]
    Zaman --> KanitMotoru
    KanitMotoru --> Cikti1[ Doğrulanmış Sonuç ]
    KanitMotoru --> Cikti2[ Belirsizlik Kararı ]
```
