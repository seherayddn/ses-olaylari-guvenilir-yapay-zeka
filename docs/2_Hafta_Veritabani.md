### 1. Veritabanı ER (Varlık-İlişki) Diyagramı
```mermaid
erDiagram
    KULLANICI ||--o{ SES_KAYDI : "yükler"
    SES_KAYDI ||--o{ ANALIZ_SONUCU : "sahip olur"
    ANALIZ_SONUCU ||--o{ AI_IDDIASI : "içerir"
    AI_IDDIASI ||--|| KANIT_DEGERLENDIRMESI : "sahip olur"
    SES_KAYDI ||--o{ OLAY_ZAMAN_DAMGASI : "barındırır"

    KULLANICI {
        int id PK
        string ad_soyad
        string rol
    }
    SES_KAYDI {
        int id PK
        int kullanici_id FK
        string dosya_adi
        float sure_sn
    }
    ANALIZ_SONUCU {
        int id PK
        int ses_kaydi_id FK
        string model_adi
        date islem_tarihi
    }
    AI_IDDIASI {
        int id PK
        int analiz_id FK
        string metin
    }
    KANIT_DEGERLENDIRMESI {
        int id PK
        int iddia_id FK
        string durum "Destekleniyor, Çelişiyor, Doğrulanamıyor"
        float belirsizlik_puani
    }
    OLAY_ZAMAN_DAMGASI {
        int id PK
        int ses_kaydi_id FK
        string olay_adi
        float baslangic
        float bitis
    }
```

### 2. Veritabanı Şeması ve Veri Sözlüğü

| Tablo Adı | Sütun Adı | Veri Tipi | Kısıtlama | Açıklama |
| :--- | :--- | :--- | :--- | :--- |
| **Kullanıcılar** | id | INT | Primary Key | Kullanıcı benzersiz kimliği |
| | ad_soyad | VARCHAR(100) | Not Null | Kullanıcının tam adı |
| | rol | VARCHAR(20) | Not Null | Admin veya Standart kullanıcı |
| **Ses_Kayitlari**| id | INT | Primary Key | Yüklenen sesin benzersiz kimliği |
| | kullanici_id | INT | Foreign Key | Sesi yükleyen kullanıcı ID'si |
| | dosya_adi | VARCHAR(255) | Not Null | Orijinal dosya adı (ör. kopek_havlamasi.wav) |
| | sure_sn | FLOAT | Not Null | Saniyesi cinsinden ses uzunluğu |
| **Yapay_Zeka_Iddialari**| id | INT | Primary Key | Modelin ürettiği cümlenin kimliği |
| | metin | TEXT | Not Null | Cümle (ör: "Köpek havlıyor") |
| **Kanit_Degerlendirmeleri**| durum | VARCHAR(50) | Not Null | "Destekleniyor", "Çelişiyor", "Doğrulanamıyor" |
| | belirsizlik_puani| FLOAT | | 0 ile 1 arası model kararsızlık skoru |
| **Olay_Zaman_Damgalari**| olay_adi | VARCHAR(100) | Not Null | Saptanan olay (ör: "Kapı Sesi") |
| | baslangic | FLOAT | Not Null | Olayın başladığı saniye |
| | bitis | FLOAT | Not Null | Olayın bittiği saniye |

### 3. Teknoloji Yığını Tablosu

| Katman | Seçilen Teknoloji | Kullanım Amacı |
| :--- | :--- | :--- |
| **İşletim Sistemi / İstemci** | Windows / Web Tarayıcı | Geliştirme ortamı ve son kullanıcı arayüzü erişimi |
| **Frontend (Arayüz)** | React.js | Dinamik ve kullanıcı dostu web arayüzünün oluşturulması |
| **Backend (Sunucu)** | FastAPI (Python) | Hızlı API uç noktalarının oluşturulması ve model yönetimi |
| **Veritabanı** | SQL Server (MSSQL) | Kullanıcı, ses ve analiz verilerinin güvenli, ilişkisel saklanması |
| **Yapay Zeka (Ses İşleme)** | librosa, PyTorch | Ses dalgalarının işlenmesi, spektrogram dönüşümleri |
| **Yapay Zeka (Modeller)** | Qwen-Audio / YAMNet (Quantize) | 4 GB VRAM kısıtına uygun ses-dil açıklaması ve zaman tespiti |
| **Prototip ve Tasarım** | Figma | Geliştirme öncesi birebir piksel mock-up ekran tasarımı |
| **Versiyon Kontrol** | Git & GitHub | Kaynak kod yönetimi ve proje dokümantasyonu |
