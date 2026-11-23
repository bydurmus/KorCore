<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Segoe+UI&weight=800&size=30&duration=2600&pause=1000&color=EDEBDC&center=true&vCenter=true&width=760&lines=KORCORE+Studio+%E2%80%94+S%C4%B1f%C4%B1rdan+Yerel+Yapay+Zeka;NO+llama.cpp+%E2%9C%97+%7C+korcore.cpp+%E2%9C%93;26B+%26+120B+Modeller+%E2%80%A2+Tek+T%C4%B1k+Kurulum+%E2%80%A2+%25100+%C3%87evrimd%C4%B1%C5%9F%C4%B1" alt="KORCORE Studio" />

<p>
  <img src="https://img.shields.io/badge/Platform-Windows%20x64%20(AVX2)-1b222a?style=for-the-badge&logo=windows&logoColor=edebdc" alt="Platform"/>
  <img src="https://img.shields.io/badge/Engine-korcore.cpp-16a34a?style=for-the-badge&logo=cplusplus&logoColor=white" alt="Engine"/>
  <img src="https://img.shields.io/badge/API-OpenAI%20%2B%20Ollama-2c3640?style=for-the-badge" alt="API"/>
  <img src="https://img.shields.io/badge/Privacy-100%25%20Offline-c99177?style=for-the-badge" alt="Offline"/>
  <a href="https://www.virustotal.com/gui/file/7e2dd3ae3a8a6d8fbd71e6710587575bc05d5b7f717d0cfc1ebdaa0e95426882?nocache=1"><img src="https://img.shields.io/badge/VirusTotal-Report-16a34a?style=for-the-badge&logo=virustotal&logoColor=white" alt="VirusTotal"/></a>
  <a href="https://korkuttech.com"><img src="https://img.shields.io/badge/Korkut%20Tech-korkuttech.com-1b222a?style=for-the-badge" alt="Korkut Tech"/></a>
</p>

**Bunca zaman dev ekran kartlarına ve bulut aboneliklerine harcanan para gerçekten şart mıydı, yoksa tek eksik düzgün bir programlama mıydı?**

[İndir](https://drive.google.com/drive/folders/1_K3rMYqzaHTXIPoesEmhpOyCb7gB0Qx_?usp=sharing) · [Kurulum](#-kurulum) · [Desteklenen Modeller](#-desteklenen-modeller) · [Ölçülen Hızlar](#-ölçülen-hız-ve-stres-testleri) · [API Uç Noktaları](#-yerel-api-uç-noktaları-12700111434) · [korkuttech.com](https://korkuttech.com)

</div>

---

## Neden KORCORE Studio?

Hazır bir motorun (`llama.cpp`, `Ollama` vb.) üzerine giydirilmiş bir arayüz değildir. Doğrudan **C++17 + CUDA** ile yazılmış bağımsız yerel motoru (`korcore.cpp`) sayesinde:

- **Sıfır Dış Bağımlılık:** Python, MinGW veya CUDA Toolkit kurulumu gerekmez; tek EXE / tek Setup ile çalışır.
- **Sıfır Bulut & Tam Gizlilik:** Tüm sohbetler, kodlar ve model dosyaları yalnızca cihazınızda (`127.0.0.1:11434`) işlenir.
- **Uygulama İçi Model İndirme:** İndirili olmayan modele listeden tıkladığınızda kaldığı yerden devam edebilen (`.part` resume destekli) yerleşik indirme çubuğuyla doğrudan iner.
- **3 Arayüz Tek Pakette:** Yerel Masaüstü Studio + Tarayıcı Web Paneli (`/chat`) + **VS Code Agent Eklentisi**.

---

## Desteklenen Modeller

| Model | Dosya | Boyut | Uygulama İçi İndirme |
| :--- | :--- | :--- | :--- |
| **Gemma-4 26B MoE (Q8)** | `gemma-4-26B-A4B-it-UD-Q8_K_XL.gguf` | `25.7 GB` | Tek tıkla listeden iner |
| **GPT-OSS 120B (F16)** | `gpt-oss-120b-F16.gguf` | `60.9 GB` | Tek tıkla listeden iner |

---

## Ölçülen Hız ve Stres Testleri

| Senaryo / Donanım | Model | Normalde (Standart Araçlar) | KORCORE Studio (`korcore.cpp`) |
| :--- | :--- | :--- | :--- |
| **Standart Laptop** *(RTX 3050 4 GB VRAM + 16 GB RAM)* | **Gemma-4 26B (Q8)** | ~1 – 3 tok/s veya VRAM yetersiz / OOM | **`10.4 tok/s`** · Önbellek isabeti: **`%99.5`** |
| **Sıkı Kısıtlı Stres Testi** *(4 GB RAM + 2 GB VRAM sınırı)* | **Gemma-4 26B (Q8)** | Çalışmaz / OOM | **`7.42 tok/s`** · RAM: `3858 MB` · VRAM: `1907 MB` |
| **Standart Laptop** *(Sıcak önbellek sonrası)* | **GPT-OSS 120B (F16)** | Standart laptopta açılmaz / OOM | **`1.9 – 5.0 tok/s`** *(60.9 GB F16 yerel üretim)* |
| **Sıkı Kısıtlı Stres Testi** *(4 GB RAM + 2 GB VRAM sınırı)* | **GPT-OSS 120B (F16)** | Çalışmaz / OOM | **`1.59 tok/s`** · RAM: `3828 MB` · VRAM: `1913 MB` |

> Not: İlk soğuk açılışta önbellek ısınırken hız daha düşük başlayabilir; 1–2 mesaj sonra sıcak önbellek devreye girer.

---

## Sistem Mimarisi

```text
+---------------------------+     +------------------------------+     +---------------------------------+     +---------------------------+
|     1. ARAYUZLER          |     |       2. YEREL API           |     |    3. KORCORE.CPP               |     |     4. DONANIM            |
+---------------------------+     +------------------------------+     +---------------------------------+     +---------------------------+
|  Masaustu Studio (Win32)  | --> |                              |     |  Akilli Bellek Yoneticisi       | <-> |  NVIDIA GPU (CUDA)        |
|  Web Paneli (/chat)       | --> |  http://127.0.0.1:11434      | --> |              |                  |     |  Sistem RAM               |
|  VS Code Agent Eklentisi  | --> |  OpenAI + Ollama Uyumlu      |     |              v                  | <-> |  NVMe SSD (.gguf & Cache) |
|                           |     |  (%100 Cihaz Ici / Gizli)    |     |  Ozel C++17 / CUDA Motoru       |     |  CPU Fallback (AVX2)      |
+---------------------------+     +------------------------------+     +---------------------------------+     +---------------------------+
```

---

## Yerel API Uç Noktaları (`127.0.0.1:11434`)

| Uç Nokta | Yöntem | Açıklama |
| :--- | :--- | :--- |
| `/v1/chat/completions` | `POST` | OpenAI uyumlu SSE akışı + yerel Tool Calling |
| `/v1/models` | `GET` | Aktif yüklü model listesi |
| `/v1/status` | `GET` | Canlı hız (`tok/s`), RAM/VRAM kullanımı ve donanım durumu |
| `/v1/stop` | `POST` | Devam eden üretimi anında durdurma |
| `/api/chat` · `/api/tags` · `/api/show` | `POST`/`GET` | Ollama uyumlu istemci desteği |
| `/chat` | `GET` | Yerleşik çoklu sohbet Web Paneli |

---

## Öne Çıkan Özellikler

- **Masaüstü & Web Paneli (`/chat`):** Çoklu sohbet geçmişi, sohbete özel sistem komutu, canlı `tok/s` & `Hit %` telemetrisi, tek tıkla `Kodu Kopyala`, yukarı kaydırma kilidi ve düşünce süreci (`Thinking`) aç/kapa.
- **VS Code Entegrasyonu:** Kurulum sırasında VS Code eklentisi otomatik kurulur; Agent modunda dosya oluşturma, düzenleme ve sınırsız süreli yerel kod üretimi desteklenir.
- **Ayarlar & Otomatik Başlatma:** Performans profilleri, RAM/VRAM/Thread kontrolü, **Windows başlangıcında çalıştırma** ve **açılışta seçili modeli otomatik başlatma**.

---

## Kurulum

[Google Drive İndirme Klasörü](https://drive.google.com/drive/folders/1_K3rMYqzaHTXIPoesEmhpOyCb7gB0Qx_?usp=sharing) üzerinden size uygun paketi indirin:

| Paket | Açıklama |
| :--- | :--- |
| **`KORCORE_Studio_Setup.exe`** | Kurulum sihirbazı, masaüstü/başlat kısayolları, Windows başlangıcı ve otomatik VS Code eklenti kurulumu. Üzerine kurulumda mevcut `models\`, sohbetler ve ayarlar **korunur**. *(Katılımsız: `/S`)* |
| **`KORCORE_Portable.zip`** | Kurulumsuz taşınabilir paket; klasöre çıkarıp `KORCORE_Studio.exe` dosyasını çalıştırın. |

### İlk Kullanım
1. `KORCORE Studio`'yu açın.
2. Sol taraftaki **Yerel Modeller** listesinden istediğiniz modele (`gemma-4` veya `gpt-oss-120b`) tıklayıp uygulama içinden indirin (veya `.gguf` dosyanızı `models\` klasörüne koyun).
3. **`Motoru Baslat`** butonuna basın ve sohbet etmeye başlayın — ya da **`Web Arayuz`** ile tarayıcıdan / **VS Code** üzerinden bağlanın.

> **Gereksinimler:** Windows 10/11 (64-bit), AVX2 destekli işlemci (2013+ Intel / AMD Ryzen). NVIDIA GPU varsa CUDA otomatik kullanılır; yoksa CPU modunda çalışır.  
> **Windows SmartScreen:** İlk açılışta uyarı çıkarsa *"Ek bilgi → Yine de çalıştır"* seçeneğini kullanabilirsiniz.

---

## Lisans

Copyright (c) 2026 [dertos.org](https://dertos.org) · [korkuttech.com](https://korkuttech.com). Tüm hakları saklıdır.

<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:1b222a,100:c99177&height=90&section=footer" width="100%" alt="footer"/>
</div>
