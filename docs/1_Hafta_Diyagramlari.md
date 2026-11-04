### 1. Kullanım Senaryosu (Use Case) Diyagramı
```mermaid
flowchart LR
    User(("Kullanıcı"))
    Admin(("Yetkili Araştırmacı"))

    UC1(["Ses Dosyası Yükleme"])
    UC2(["Analiz Sürecini Başlatma"])
    UC3(["Zaman Çizelgesini İnceleme"])
    UC4(["Model İddialarını ve Kanıtları İnceleme"])
    UC5(["Deney Sonuçlarını Listeleme"])
    UC6(["Model Karşılaştırmalarını İnceleme"])

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
        UI["React Frontend / Figma Mockup Birebir"]
    end

    subgraph Sunucu_Backend
        API["FastAPI Backend"]
    end

    subgraph Yapay_Zeka_Katmanı
        AudioProc["Ses Ön İşleme"]
        BaseModel["Temel Ses-Dil Modeli"]
        SED["Ses Olay Tespiti"]
        ECHO["ECHO Kanıt Değerlendirici"]
    end

    subgraph Veritabanı
        DB[("SQL Server")]
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
    A["Ses Yüklendi"] --> B{"Boyut Uygun mu?"}
    B -- Hayır --> C["Hata: Dosya Çok Büyük"]
    B -- Evet --> D{"Format WAV/MP3 mü?"}
    D -- Hayır --> E["Hata: Desteklenmeyen Format"]
    D -- Evet --> F["librosa ile Oku"]
    F --> G{"Dosya Bozuk mu?"}
    G -- Evet --> H["Hata: Okunamadı"]
    G -- Hayır --> I["Analize Hazır"]
```

**Akış 2: ECHO Kanıt Eşleştirme**
```mermaid
graph TD
    A["Model Açıklama Üretti"] --> B["Açıklamayı İddialara Böl"]
    B --> C["Olay Zaman Damgalarını Al"]
    C --> D["İddia ve Kanıtı Eşleştir"]
    D --> E{"Kanıt Destekliyor mu?"}
    E -- Evet --> F["Sonuç: Destekleniyor"]
    E -- Hayır --> G["Sonuç: Çelişiyor"]
    E -- Kanıt Yok --> H["Sonuç: Doğrulanamıyor"]
```

**Akış 3: Belirsizlik Kararı**
```mermaid
graph TD
    A["İddia Durumu: Doğrulanamıyor"] --> B["Belirsizlik Puanı Hesapla"]
    B --> C{"Puan Eşiği Aştı mı?"}
    C -- Hayır --> D["Kullanıcıya Temkinli Göster"]
    C -- Evet --> E["Modeli Sustur / Cevap Verme"]
    E --> F["Uyarı: Yeterli Kanıt Bulunamadı"]
```

### 4. AI Pipeline
```mermaid
graph LR
    HamSes["Ham Ses"] --> M1["Ses-Dil Modeli"]
    HamSes --> M2["Ses Olay Tespiti"]
    M1 --> İddialar["Metin İddiaları"]
    M2 --> Zaman["Zaman Damgaları"]
    İddialar --> KanitMotoru["Kanıt Motoru"]
    Zaman --> KanitMotoru
    KanitMotoru --> Cikti1["Doğrulanmış Sonuç"]
    KanitMotoru --> Cikti2["Belirsizlik Kararı"]
```
