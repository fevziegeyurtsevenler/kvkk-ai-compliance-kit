<p align="center"><img src="assets/banner.svg" alt="kvkk-ai-compliance-kit" width="100%"></p>

# kvkk-ai-compliance-kit

<p align="center">
  <b>LLM ve yapay zeka uygulamaları için KVKK uyum kiti.</b><br>
  Pratik kontrol listesi, 18 PII tipi taksonomisi, maskeleme reçeteleri ve
  DPIA-benzeri risk kontrolleri — <b>Türkçe</b>, kod dostu, üretime dönük.
</p>

<p align="center">
  <a href="LICENSE"><img alt="License: CC BY 4.0" src="https://img.shields.io/badge/license-CC%20BY%204.0-blue.svg"></a>
  <img alt="Dil: Türkçe" src="https://img.shields.io/badge/dil-T%C3%BCrk%C3%A7e-red.svg">
  <img alt="KVKK Kanun No. 6698" src="https://img.shields.io/badge/KVKK-6698-informational.svg">
  <img alt="OWASP LLM Top 10 (2025)" src="https://img.shields.io/badge/OWASP-LLM%20Top%2010%20(2025)-orange.svg">
  <img alt="EU AI Act 2024/1689" src="https://img.shields.io/badge/EU%20AI%20Act-2024%2F1689-9cf.svg">
  <img alt="MITRE ATLAS" src="https://img.shields.io/badge/MITRE-ATLAS-lightgrey.svg">
  <img alt="Hukuki tavsiye değildir" src="https://img.shields.io/badge/uyar%C4%B1-hukuki%20tavsiye%20de%C4%9Fildir-critical.svg">
  <img alt="PR'lar açık" src="https://img.shields.io/badge/PR-a%C3%A7%C4%B1k-brightgreen.svg">
</p>

---

> [!WARNING]
> **Bu depo hukuki tavsiye değildir.** Buradaki kontrol listeleri, taksonomi ve
> kod örnekleri; mühendis ekiplerinin KVKK (Kanun No. 6698) ve yapay zeka
> düzenlemeleriyle ilgili konuşabilmesi için hazırlanmış **teknik bir başlangıç
> setidir**. Bağlayıcı bir uyum değerlendirmesi için bir avukata / veri koruma
> danışmanına başvurun. Kanun metninin bağlayıcı kaynağı her zaman
> [mevzuat.gov.tr](https://www.mevzuat.gov.tr/) ve
> [KVKK Kurumu](https://www.kvkk.gov.tr/)'dur.

---

## Neden bu depo?

Bir LLM uygulaması kişisel veriyi üç yeni yüzeyde sızdırır ve bu yüzeyler
klasik uyum kontrol listelerinde yoktur:

1. **Prompt / bağlam penceresi** — kullanıcının yapıştırdığı TCKN, IBAN, sağlık
   notu doğrudan modele gider.
2. **Loglar ve trace'ler** — istem/yanıt logları, gözlemlenebilirlik (tracing)
   araçları ve hata raporları ham PII'yi kalıcılaştırır.
3. **Eğitim / ince ayar (fine-tuning) verisi** — üretim konuşmaları geri besleme
   döngüsüne girdiğinde kişisel veri modele gömülür.

Bu kit; **hangi veriyi topladığını**, **nasıl azalttığını (minimizasyon)**,
**nasıl maskelediğini** ve **ne kadar sakladığını** her biri bir KVKK maddesine
bağlanmış maddeler halinde denetlemeni sağlar.

### Kapsamda olan / olmayan

| ✅ Kapsamda | ❌ Kapsam dışı |
|---|---|
| Teknik kontrol listesi (checklist) | Hukuki mütalaa / avukatlık hizmeti |
| 18 PII tipi taksonomisi + tespit ipuçları | VERBİS kaydını senin yerine yapmak |
| Maskeleme / tokenizasyon reçeteleri | Aydınlatma metni / sözleşme şablonu (hukuki metin) |
| DPIA-benzeri risk soruları | "Uyumlu" sertifikası veya garanti |
| OWASP LLM Top 10 & ATLAS eşlemesi | AB AI Act uygunluk beyanı (DoC) üretimi |

---

## İçindekiler

- [Depo yapısı](#depo-yap%C4%B1s%C4%B1)
- [18 PII tipi taksonomisi](#18-pii-tipi-taksonomisi)
- [KVKK uyum kontrol listesi](#kvkk-uyum-kontrol-listesi)
- [Veri minimizasyonu](#1-veri-minimizasyonu-madde-4)
- [Aydınlatma](#2-ayd%C4%B1nlatma-madde-10)
- [Saklama ve imha](#3-saklama-ve-imha-madde-4--7--%C4%B0mha-y%C3%B6netmeli%C4%9Fi)
- [Maskeleme reçeteleri](#maskeleme-re%C3%A7eteleri)
- [Log kontrolleri](#log-kontrolleri)
- [DPIA-benzeri risk kontrolleri](#dpia-benzeri-risk-kontrolleri)
- [OWASP LLM Top 10 (2025) eşlemesi](#owasp-llm-top-10-2025-e%C5%9Flemesi)
- [MITRE ATLAS notu](#mitre-atlas-notu)
- [AB AI Act kesişimi](#ab-ai-act-kesi%C5%9Fimi)
- [Ekosistem](#ekosistem)
- [Katkı & lisans](#katk%C4%B1)

---

## Depo yapısı

```
kvkk-ai-compliance-kit/
├─ README.md                     ← bu dosya (özet + hızlı başvuru)
├─ checklists/
│  ├─ kvkk-llm-checklist.md      ← madde madde denetim listesi
│  ├─ dpia-benzeri-sorular.md    ← risk değerlendirme soru seti
│  └─ log-guvenligi.md           ← log/trace özelinde kontroller
├─ taxonomy/
│  └─ pii-18.md                  ← 18 PII tipi, tespit ipuçları, hassasiyet
├─ masking/
│  ├─ recipes.md                 ← maskeleme kalıpları (dil-bağımsız)
│  └─ examples/mask.py           ← referans Python fonksiyonları
└─ mappings/
   ├─ owasp-llm-top10-2025.md    ← PII riskleri ↔ OWASP eşlemesi
   ├─ mitre-atlas.md             ← ilgili ATLAS taktik/teknikleri
   └─ eu-ai-act.md               ← KVKK ↔ AB AI Act kesişim notu
```

> Aşağıdaki bölümler bu dosyaların özetidir; tam sürümler ilgili klasörlerdedir.

---

## 18 PII tipi taksonomisi

LLM uygulamalarında en sık karşılaşılan 18 kişisel veri tipi. **Özel nitelikli**
işaretli olanlar KVKK **Madde 6** kapsamındadır ve daha katı işleme koşullarına
tabidir.

| # | PII tipi | Örnek biçim | Hassasiyet | Not |
|---|----------|-------------|:----------:|-----|
| 1 | T.C. Kimlik No (TCKN) | `12345678901` (11 hane) | Yüksek | Algoritmayla doğrulanabilir; hash yerine tokenizasyon önerilir |
| 2 | Ad-Soyad | `Ayşe Yılmaz` | Orta | Tek başına dolaylı; başka alanla birleşince tanımlayıcı |
| 3 | Telefon (GSM) | `+90 5XX XXX XX XX` | Orta | |
| 4 | E-posta | `ad@alan.com` | Orta | Yerel kısım ad-soyad içerebilir |
| 5 | Açık adres | mahalle/sokak/no | Yüksek | Konumla birlikte yüksek risk |
| 6 | IBAN / banka hesabı | `TR` + 24 hane | Yüksek | Finansal |
| 7 | Kredi kartı (PAN) | 13–19 hane, Luhn | Yüksek | PCI DSS ayrıca uygulanır |
| 8 | Pasaport no | `U01234567` | Yüksek | |
| 9 | Sürücü belgesi no | ülkeye göre değişir | Yüksek | |
| 10 | Araç plakası | `34 ABC 123` | Orta | Kişiyle ilişkilenebilir |
| 11 | Doğum tarihi | `GG.AA.YYYY` | Orta | Yaş/kimlik türetimi |
| 12 | IP adresi | IPv4 / IPv6 | Orta | KVKK ve AB pratiğinde kişisel veri sayılabilir |
| 13 | Cihaz kimliği / MAC | `AA:BB:CC:...` | Orta | Kalıcı tanımlayıcı |
| 14 | Coğrafi konum | enlem/boylam | Yüksek | Hareket örüntüsü |
| 15 | Sosyal güvenlik (SGK) | sicil / no | Yüksek | |
| 16 | Vergi kimlik no (VKN) | 10 hane | Orta | Tüzel/gerçek kişi |
| 17 | Sağlık verisi | tanı, ilaç, rapor | **Özel nitelikli (M.6)** | Rıza/istisna zorunlu |
| 18 | Biyometrik / genetik | yüz, parmak izi, DNA | **Özel nitelikli (M.6)** | En katı rejim |

> [!NOTE]
> Regex tabanlı tespit **yanlış pozitif/negatif** üretir (ör. TCKN uzunluğunda
> herhangi bir 11 haneli sayı). Üretimde regex'i sağlama (Luhn, TCKN algoritması)
> ve bağlam sinyalleriyle birleştir; tek başına regex'e uyum garantisi olarak
> güvenme. Örüntüler `taxonomy/pii-18.md` içindedir.

---

## KVKK uyum kontrol listesi

KVKK'nın (Kanun No. 6698, RG 07.04.2016) temel maddelerine göre gruplanmıştır.
Tam sürüm: `checklists/kvkk-llm-checklist.md`.

### 1. Veri minimizasyonu (Madde 4)

Madde 4 genel ilkeleri: *hukuka ve dürüstlük kurallarına uygunluk; doğruluk ve
güncellik; belirli, açık ve meşru amaç; **amaçla bağlantılı, sınırlı ve
ölçülülük**; gerekli süre kadar saklama.*

- [ ] Modele giden prompt'ta yalnızca **amaç için gerekli** alanlar var (tüm
      kullanıcı kaydı değil).
- [ ] Sistem promptu ve RAG bağlamı, gerekmeyen PII içeren belgeleri sızdırmıyor.
- [ ] Serbest metin alanlarına yapıştırılan PII, modele gitmeden önce
      **maskeleniyor / redakte ediliyor**.
- [ ] Fonksiyon/araç (tool) çağrılarının argümanları en az PII ile sınırlı.
- [ ] İnce ayar (fine-tune) veri seti PII'den arındırılmış veya
      anonimleştirilmiş.

### 2. Aydınlatma (Madde 10)

Veri sorumlusunun kimliği, işleme amaçları, aktarılan taraflar ve ilgili kişinin
hakları hakkında **aydınlatma yükümlülüğü** (Madde 10). *Not: veri güvenliği
yükümlülükleri ayrıca Madde 12'dedir.*

- [ ] Kullanıcıya **verinin bir yapay zeka modeline gönderildiği** açıkça
      bildiriliyor.
- [ ] Üçüncü taraf model sağlayıcı (OpenAI, Anthropic, Google vb.) ve varsa
      **yurt dışı aktarım** aydınlatma metninde belirtiliyor.
- [ ] Kullanıcının erişim/silme/itiraz hakları (Madde 11) için işleyen bir kanal
      var.
- [ ] Açık rıza gereken hallerde rıza; değilse dayanılan işleme şartı (Madde 5/6)
      kayıt altında.

### 3. Saklama ve imha (Madde 4 & 7 + İmha Yönetmeliği)

- [ ] Prompt/yanıt logları için **saklama süresi** tanımlı ve süre sonunda
      silme/yok etme/anonimleştirme (Madde 7) otomatik.
- [ ] Model sağlayıcının veri saklama / eğitimde kullanma politikası
      (ör. "zero-retention" / no-training) sözleşmeyle netleştirilmiş.
- [ ] Yedekler ve gözlemlenebilirlik (tracing) deposu da imha politikasına dahil.
- [ ] VERBİS'e kayıtlıysanız **Saklama ve İmha Politikası** güncel.

---

## Maskeleme reçeteleri

Amaç: PII'yi log/prompt içinde **geri döndürülemez** biçimde azaltmak, ama gerekli
hallerde biçim doğrulamayı korumak. Tam liste: `masking/recipes.md`.

| PII | Girdi | Maskeli çıktı | Yöntem |
|-----|-------|---------------|--------|
| TCKN | `12345678901` | `123******01` | ilk 3 + son 2, ortası maske |
| Telefon | `+90 532 123 45 67` | `+90 5** *** ** 67` | ülke kodu + son 2 |
| E-posta | `ayse.yilmaz@firma.com` | `a***@f****.com` | ilk harf + TLD |
| IBAN | `TR33 0006 1005 1978 6457 8413 26` | `TR** **** ... **** **26` | son 2 |
| Kart (PAN) | `4111 1111 1111 1111` | `**** **** **** 1111` | yalnız son 4 (PCI hizası) |
| IP | `192.168.10.42` | `192.168.*.*` | son iki oktet |

```python
# masking/examples/mask.py — referans (bağımlılıksız)
def mask_tckn(v: str) -> str:
    d = "".join(ch for ch in v if ch.isdigit())
    return f"{d[:3]}{'*'*6}{d[-2:]}" if len(d) == 11 else "***********"

def mask_email(v: str) -> str:
    local, _, domain = v.partition("@")
    if not domain:
        return "***"
    name = domain.rsplit(".", 1)
    tld = name[-1] if len(name) > 1 else ""
    return f"{local[:1]}***@{name[0][:1]}****.{tld}"

def mask_pan(v: str) -> str:            # PCI DSS: en fazla son 4 açık
    d = "".join(ch for ch in v if ch.isdigit())
    return f"{'*'*(len(d)-4)}{d[-4:]}" if len(d) >= 4 else "****"
```

> [!IMPORTANT]
> **Maskeleme ≠ anonimleştirme.** Geri döndürülebilir maskeleme (ör. tokenizasyon
> haritası saklanıyorsa) hâlâ kişisel veridir ve KVKK kapsamındadır. Yalnızca
> **geri döndürülemez** biçimde tanımlanamaz hale getirilmiş veri KVKK **Madde
> 28** anlamında anonim sayılabilir. Hangi eşiğin karşılandığına hukuki/teknik
> değerlendirmeyle karar verin.

---

## Log kontrolleri

`checklists/log-guvenligi.md` özeti:

- [ ] İstem (prompt) ve yanıt logları **maskeleme katmanından** geçiyor.
- [ ] Gözlemlenebilirlik / trace araçları (LangSmith, Langfuse, Phoenix, OTel
      vb.) ham PII'yi dışarı kaçırmıyor; PII redaksiyonu sağlayıcı tarafında da
      açık.
- [ ] Hata/exception raporlarında (Sentry vb.) prompt gövdesi maskeli.
- [ ] Log erişimi yetkiyle sınırlı; log saklama süresi imha politikasıyla uyumlu.
- [ ] Model sağlayıcıya giden isteklerde başlık/metadata'da gereksiz PII yok.

---

## DPIA-benzeri risk kontrolleri

> **Dürüstlük notu:** GDPR **Madde 35** biçimsel bir **DPIA** (Veri Koruma Etki
> Değerlendirmesi) zorunlu kılar. KVKK'da birebir aynı zorunluluk **yoktur**;
> ancak Kurul rehberleri ve iyi uygulama, yüksek riskli işlemeler için risk
> değerlendirmesi önerir. Aşağıdaki set bu nedenle "DPIA-**benzeri**"dir.

`checklists/dpia-benzeri-sorular.md` içinden örnek sorular:

- İşleme, ilgili kişiler üzerinde **yüksek risk** doğuruyor mu (özel nitelikli
  veri, sistematik profil çıkarma, otomatik karar)?
- Model çıktısı bir kişi hakkında **karar** üretiyor mu (kredi, işe alım, sağlık
  triyajı)? Öyleyse insan denetimi ve itiraz yolu var mı?
- Yurt dışı aktarım hangi **Madde 9** dayanağıyla yapılıyor (2024 değişikliği;
  Kanun No. 7499, yürürlük 01.06.2024)?
- Prompt injection ile model, yetkisi olmayan PII'ye erişip sızdırabilir mi?
  (Bkz. OWASP LLM01 / LLM02.)

---

## OWASP LLM Top 10 (2025) eşlemesi

Bu kitteki PII riskleri, [OWASP Top 10 for LLM Applications 2025](https://genai.owasp.org/llm-top-10/)
ile şu şekilde kesişir:

| OWASP 2025 | İlgili kontrol |
|---|---|
| **LLM01: Prompt Injection** | Maskeleme + araç yetkisi sınırlama; injection ile PII sızdırma senaryoları |
| **LLM02: Sensitive Information Disclosure** | 18 PII taksonomisi, maskeleme, log kontrolleri — bu kitin çekirdeği |
| **LLM03: Supply Chain** | Model/sağlayıcı veri politikaları; eklenti güvenliği (bkz. ekosistem) |
| **LLM04: Data and Model Poisoning** | Fine-tune verisinin PII'den arındırılması ve kaynak doğrulama |
| **LLM06: Excessive Agency** | Araç/fonksiyon çağrılarında en az PII ilkesi |
| **LLM07: System Prompt Leakage** | Sistem promptunda PII/sır bulundurmama |

> `mappings/owasp-llm-top10-2025.md` tam listeyi içerir (LLM01–LLM10).

## MITRE ATLAS notu

[MITRE ATLAS](https://atlas.mitre.org/) (Adversarial Threat Landscape for
Artificial-Intelligence Systems), yapay zeka sistemlerine yönelik saldırgan
taktik ve tekniklerini tanımlayan bir bilgi tabanıdır. PII sızıntısı açısından
en ilgili taktikler **Exfiltration** ve **ML Attack Staging**'tir; kit,
tespit/maskeleme kontrollerini bu taktiklerle `mappings/mitre-atlas.md` içinde
ilişkilendirir. (ATLAS teknik ID'leri zamanla güncellendiğinden, kullanmadan
önce güncel ID'yi ATLAS sitesinden doğrulayın.)

## AB AI Act kesişimi

[Regulation (EU) 2024/1689 (AI Act)](https://eur-lex.europa.eu/eli/reg/2024/1689/oj)
KVKK'dan ayrı, **ürün güvenliği** eksenli bir düzenlemedir. Önemli tarihler:

| Tarih | Yürürlüğe giren |
|---|---|
| 1 Ağustos 2024 | Yürürlüğe giriş |
| 2 Şubat 2025 | Yasaklı uygulamalar + AI okuryazarlığı |
| 2 Ağustos 2025 | Genel amaçlı AI (GPAI) yükümlülükleri |
| 2 Ağustos 2026 | Yüksek riskli sistemlerin (Annex III) çoğu yükümlülüğü |
| 2 Ağustos 2027 | Belirli ürün-gömülü yüksek riskli sistemler (Annex I) |

> **Türkiye AB üyesi değildir**, ancak AI Act **ülke-dışı (extraterritorial)**
> etkiye sahiptir (Art. 2): çıktısı AB'de kullanılan veya AB pazarına sistem
> sunan Türk şirketleri kapsam altına girebilir. KVKK ile AI Act **birlikte**
> değerlendirilmelidir; biri diğerinin yerine geçmez. Detay:
> `mappings/eu-ai-act.md`.

---

## Ekosistem

Bu kit, LLM/agent güvenliği üzerine daha geniş bir açık kaynak setinin parçasıdır:

| Depo | Ne işe yarar |
|---|---|
| [**uncloak**](https://github.com/fevziegeyurtsevenler/uncloak) | AI agent eklentilerinde gizli prompt injection & tedarik zinciri tarayıcısı (Skills, MCP, kural dosyaları) |
| [**skills-in-the-wild**](https://github.com/fevziegeyurtsevenler/skills-in-the-wild) | Gerçek dünya agent Skill'lerinin güvenlik gözlemleri |
| [**awesome-agent-supply-chain-security**](https://github.com/fevziegeyurtsevenler/awesome-agent-supply-chain-security) | Agent tedarik zinciri güvenliği için derli toplu kaynak listesi |
| [**llm-security-skills**](https://github.com/fevziegeyurtsevenler/llm-security-skills) | LLM güvenlik iş akışları için Claude/agent Skill'leri |
| [**prompt-injection-corpus**](https://github.com/fevziegeyurtsevenler/prompt-injection-corpus) | Çok dilli prompt injection örnek korpusu |

---

## Katkı

Yeni PII tipi, maskeleme kalıbı, kontrol maddesi veya eşleme düzeltmesi için
PR'lar açıktır. Lütfen `CONTRIBUTING.md`'yi okuyun ve **gerçek örnek PII
göndermeyin** — yalnızca sentetik/kurgusal veri kullanın.

## Lisans

İçerik **[CC BY 4.0](LICENSE)**, kod örnekleri **MIT** altında dağıtılır.
Atıf: *Fevzi Ege Yurtsevenler — AltaySec*.

---

<sub>
Hazırlayan: <b>Fevzi Ege Yurtsevenler</b> — çok dilli LLM/AI güvenlik araştırmacısı,
OWASP GenAI merged contributor. Bu depo bir uyum <b>başlangıç kitidir</b>, hukuki
danışmanlık veya uyum garantisi değildir. Kanunların bağlayıcı kaynağı
<a href="https://www.kvkk.gov.tr/">KVKK Kurumu</a> ve
<a href="https://www.mevzuat.gov.tr/">mevzuat.gov.tr</a>'dir.
</sub>

---

## İlgili AltaySec Kaynakları

- 📖 [KVKK ve LLM Güvenliği: Yapay Zekâ Veri Uyumu](https://altaysec.com.tr/arastirmalar/kvkk-llm-guvenligi) — konunun derinlemesine Türkçe analizi
- 🌐 [AltaySec Araştırmalar](https://altaysec.com.tr/arastirmalar/) — Türkçe yapay zekâ güvenliği yazıları

## Atıf

```bibtex
@software{altaysec_kvkk_ai_compliance_kit_2026,
  author = {{AltaySec}},
  title  = {kvkk-ai-compliance-kit},
  year   = {2026},
  url    = {https://github.com/fevziegeyurtsevenler/kvkk-ai-compliance-kit}
}
```
