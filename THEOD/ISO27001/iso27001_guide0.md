# ISO/IEC 27001 нэвтрүүлэх бүрэн гарын авлага
### Бодит технологийн компани дээр эхнээс нь сертификат авах хүртэл

> **Тайлбар:** Энэхүү гарын авлага нь зөвхөн онол биш, бодит SaaS/Cloud компанийн жишээн дээр ISMS (Information Security Management System)-ийг эхнээс нь байгуулж, сертификат авах хүртэлх бүх үе шатыг өдөр тутмын ажил, уулзалт, баримт бичиг, тикет, лог, нотолгооны түвшинд дүрсэлсэн болно. Зохиогч нь ISO/IEC 27001-ийн ахлах зөвлөх, аудитор, мэдээллийн аюулгүй байдлын архитекторын байр сууринаас бичив.

---

## Бидний жишээ компани: CloudNova

Энэ гарын авлагад бид **CloudNova LLC** хэмээх зохиомол боловч бодит амьдралд маш ойр компанийг авч үзнэ. CloudNova нь APAC бүс нутгийн дунд хэмжээний бизнесүүдэд зориулсан **HR ба цалингийн SaaS платформ** нийлүүлдэг. Жилийн орлого ойролцоогоор 25 сая ам.доллар, харилцагч нь 1,200 гаруй байгууллага, эдгээрийн нийт 400,000 гаруй ажилтны хувийн болон цалингийн мэдээллийг боловсруулдаг.

Бид түүхэн дэх дүрүүдийг танилцуулъя — энэ гарын авлагын туршид эдгээр хүмүүс байнга гарч ирнэ:

- **Энхбат (CISO)** — Мэдээллийн аюулгүй байдлын дарга. Төслийн гол ивээн тэтгэгч (sponsor).
- **Сараа (ISMS Manager / GRC Lead)** — ISMS-ийг өдөр тутам удирддаг хүн. Баримт бичиг, эрсдэл, аудитыг хариуцна.
- **Батбаяр (IT / SysAdmin Lead)** — Active Directory, Microsoft 365, эндпойнтыг хариуцна.
- **Тэмүүлэн (DevOps Lead)** — AWS дэд бүтэц, CI/CD, secrets, дэд бүтцийн аюулгүй байдал.
- **Болор (SOC Analyst)** — SIEM, лог, мониторинг, incident-ийг хариуцна.
- **Оюун (Internal Auditor)** — Дотоод аудит хийнэ (хараат бус байх ёстой).
- **Мөнхзул (Хууль/Compliance)** — Гэрээ, нийлүүлэгчийн үнэлгээ, GDPR/хувийн нууц.
- **Дэлгэр (CEO)** — Гүйцэтгэх захирал, удирдлагын зүгээс баталгаа гаргана.

---

## 1. Компанийн танилцуулга

ISMS байгуулахын өмнө бид компанийн "одоогийн байдлыг" (as-is state) бүрэн зурах ёстой. Сараа төслийн эхний долоо хоногт **Батбаяр, Тэмүүлэн** нартай хамтран дараах зургийг гаргасан.

### Компанийн хэмжээ

- Нийт **300 ажилтан**: Engineering 120, Product 25, Sales 45, Customer Success 40, Finance/HR/Admin 35, Security/IT 20, бусад 15.
- Оффис: Улаанбаатарт төв оффис (180 хүн), Сингапурт борлуулалтын оффис (40 хүн), үлдсэн нь алсын зайнаас (remote) ажилладаг.
- Ажлын хэв маяг: Hybrid. BYOD хэсэгчилэн зөвшөөрдөг (харин кодын орчинд биш).

### Сүлжээний бүтэц

- Оффисын сүлжээ нь **Meraki MX** firewall-аар хамгаалагдсан, VLAN сегментчлэлтэй:
  - `VLAN 10` — Corporate (ажилтны лаптоп)
  - `VLAN 20` — Guest Wi-Fi (бүрэн тусгаарлагдсан)
  - `VLAN 30` — Server/Management (зөвхөн IT)
  - `VLAN 40` — VoIP
- Дотоод болон AWS хооронд **Site-to-Site VPN** болон зарим тохиолдолд **AWS Direct Connect**.
- Алсын ажилтнууд **WireGuard/Tailscale** дээр суурилсан Zero Trust VPN-ээр дотоод нөөцөд ханддаг.

### Серверүүд

CloudNova нь "cloud-first" компани боловч хэдэн дотоод сервертэй:

- **Дотоод (on-prem):** Active Directory Domain Controller x2, файл сервер, принтер сервер, нэг хуучин ERP. Бүгд VMware ESXi дээр.
- **AWS дээрх production:** Дараагийн хэсэгт дэлгэрэнгүй.

### Cloud орчин (AWS гол үүлэн платформ)

CloudNova-ийн production бүхэлдээ **AWS** дээр (ap-southeast-1 Singapore, ap-east-1 Hong Kong region-уудад):

- **Compute:** EKS (Kubernetes) кластер, зарим хуучин үйлчилгээ EC2 дээр, шинэ микросервисүүд Fargate дээр.
- **Database:** Amazon RDS (PostgreSQL, Multi-AZ), Amazon DynamoDB, ElastiCache (Redis).
- **Storage:** S3 (хэрэглэгчийн файл, нөөцлөлт, лог), EBS.
- **Networking:** VPC, олон subnet, ALB, WAF, CloudFront CDN.
- **Identity:** AWS IAM, IAM Identity Center (SSO), AWS Organizations (олон данс — prod, staging, dev, security, logging тус бүр тусдаа данс).
- **Security services:** GuardDuty, AWS Config, CloudTrail, Security Hub, KMS, Secrets Manager.

### Ашиглаж буй системүүд (SaaS stack)

- **Identity/Productivity:** Microsoft 365 (Entra ID, Exchange, SharePoint, Teams).
- **Source control & CI/CD:** GitHub Enterprise, GitHub Actions.
- **HR/Internal:** BambooHR.
- **CRM:** Salesforce.
- **Support:** Zendesk.
- **Project mgmt:** Jira, Confluence.
- **Communication:** Slack.
- **Monitoring/Observability:** Datadog, Sentry.
- **SIEM:** Splunk Cloud (зарим компани Microsoft Sentinel сонгодог — энд Splunk гэж үзье).
- **EDR:** CrowdStrike Falcon.
- **MDM:** Microsoft Intune (Windows), Jamf (Mac).
- **Secrets/PAM:** HashiCorp Vault + AWS Secrets Manager, CyberArk зарим production-д.

### Хэрэглэгчдийн мэдээлэл (хамгийн эмзэг хэсэг)

CloudNova нь дараах **хувийн болон эмзэг мэдээллийг** боловсруулдаг:

- Ажилтны нэр, регистрийн дугаар, банкны данс (цалин шилжүүлэх).
- Цалин, татвар, нийгмийн даатгал.
- Гэр бүлийн мэдээлэл (тэтгэмжийн хувьд).
- Зарим тохиолдолд эрүүл мэндийн даатгалын мэдээлэл.

Энэ нь компанийг **GDPR, Монголын Хувь хүний нууцыг хамгаалах тухай хууль, Сингапурын PDPA** зэрэгт хамаатуулдаг тул мэдээллийн аюулгүй байдал нь "сонголт" биш "амин чухал" асуудал болдог.

### Өгөгдлийн урсгал (data flow)

Сараа төслийн эхэнд **Data Flow Diagram (DFD)** зурсан. Энгийн урсгал:

```
[Харилцагчийн ажилтан] 
      │ HTTPS (TLS 1.3)
      ▼
[CloudFront CDN] → [AWS WAF] → [ALB]
      ▼
[EKS дээрх API үйлчилгээ]
      ├──→ [RDS PostgreSQL]  (encrypted at rest, KMS)
      ├──→ [S3 файл хадгалалт] (SSE-KMS)
      ├──→ [Redis cache]
      └──→ [Гуравдагч тал: цалин шилжүүлэх банкны API]
```

Энэ диаграм нь хожим **scope, asset inventory, risk assessment**-ийн үндэс болно. ISO 27001-д "юу хамгаалах вэ" гэдгийг мэдэхгүйгээр эхэлж болохгүй — иймээс энэ нь хамгийн эхний бөгөөд хамгийн чухал ажил.

> **Бодит артефакт:** Энэ бүх мэдээлэл Confluence дээр "**CloudNova — Current State Architecture**" нэртэй хуудас болон Lucidchart дээрх DFD болж хадгалагдсан. Энэ нь аудиторт өгөгдөх анхны нотолгоонуудын нэг.

---

## 2. ISO 27001 хэрэгжүүлэх шийдвэр гарсан үе

### Яагаад хэрэгжүүлэх болсон бэ?

CloudNova-д ISO 27001 хийх шийдвэр гэнэт гараагүй. Дараах **бодит шалтгаанууд** хуримтлагдсан:

1. **Том харилцагчийн шаардлага.** Сингапурын нэг банк CloudNova-тай гэрээ хийхдээ "ISO 27001 сертификаттай юу?" гэж асуусан. Сертификатгүй учир 1.2 сая долларын гэрээ зогссон. Энэ нь хамгийн хүчтэй "business driver" болсон.
2. **Vendor security questionnaire-ийн ачаалал.** Sales баг сар бүр 10-15 харилцагчийн аюулгүй байдлын асуулга бөглөж, бүгд өөр өөр — ISO 27001 нь эдгээрийг нэг стандарт хариулт болгох боломж олгоно.
3. **Эрсдэлийн өсөлт.** Өрсөлдөгч компани дата алдалт (breach) гаргаж, зах зээлд айдас төрсөн.
4. **Хөрөнгө оруулагчийн шаардлага.** Series B-ийн өмнө due diligence-д аюулгүй байдлын төлөвшил шаардсан.

### Удирдлагын хурал (Management kickoff)

3-р сарын эхээр CEO Дэлгэр удирдлагын хурал зарлав. Оролцогчид: CEO, CISO Энхбат, CFO, CTO, Хуулийн Мөнхзул. Хурлаар:

- Энхбат ISO 27001-ийн ач холбогдол, зардал (зөвлөх, гэрчилгээжүүлэх байгууллага, хэрэгслүүд, хүний цаг), хугацааг танилцуулсан. Тооцоолсон төсөв: ~$120,000 (зөвлөх + аудит + хэрэгсэл) + дотоод хүний 9 сарын хүчин чармайлт.
- **Шийдвэр:** ISO 27001:2022 хувилбараар 9-12 сарын дотор сертификат авах.
- **Гол шийдвэр — Leadership commitment.** ISO 27001-ийн Clause 5 (Leadership) шаардлагын дагуу удирдлага нь зөвхөн төсөв батлаад зогсохгүй идэвхтэй оролцох ёстой. Энэ хурлын тэмдэглэл нь өөрөө **аудитын нотолгоо** болно.

### Ямар зорилго тавьсан бэ?

SMART зорилгууд:

- 12 сарын дотор ISO 27001:2022 сертификат авах.
- Vendor security questionnaire-т зарцуулах хугацааг 70%-иар бууруулах.
- Критик эрсдэлийн тоог "хүлээн зөвшөөрөгдөх" түвшинд хүртэл бууруулах.
- 100% ажилтан аюулгүй байдлын сургалт дамжих.

### Ямар баг байгуулагдсан бэ?

Энхбат **ISMS Steering Committee** болон **ажлын баг** гэсэн хоёр түвшний бүтэц байгуулсан:

**Steering Committee (улирал бүр уулзана):** CEO, CISO, CTO, CFO, Хууль. Энэ нь стратегийн шийдвэр, төсөв, эрсдэл хүлээн зөвшөөрөх эрх мэдэлтэй.

**ISMS Project Team (долоо хоног бүр уулзана):**
- Сараа — ISMS Manager (төслийн менежер, гол хариуцагч).
- Батбаяр — IT controls.
- Тэмүүлэн — Cloud/DevOps controls.
- Болор — Monitoring/Incident.
- Мөнхзул — Legal/Supplier.
- Оюун — Internal audit (ХАРАА БУС — өөрийн хэрэгжүүлсэн зүйлээ аудит хийхгүй).

### Ямар үүрэг хариуцлага хуваарилсан бэ? (RACI)

Сараа **RACI матриц** гаргасан. Жишээ хэсэг:

| Үйл ажиллагаа | CISO | ISMS Mgr | IT | DevOps | SOC | Auditor |
|---|---|---|---|---|---|---|
| ISMS Policy батлах | A | R | C | C | C | I |
| Risk Assessment | A | R | C | C | C | I |
| Access Control хэрэгжүүлэх | A | C | R | C | I | I |
| AWS hardening | A | C | I | R | C | I |
| SIEM/Monitoring | A | C | I | C | R | I |
| Internal Audit | I | C | I | I | I | R |

(R=Responsible, A=Accountable, C=Consulted, I=Informed)

> **Бодит артефакт:** Энэ үе шатад үүсэх баримтууд — Management kickoff meeting minutes, ISMS Project Charter (Confluence), RACI matrix (Google Sheets), Project plan (Jira дээр "ISO27001" нэртэй epic, 200+ task). Эдгээр нь Clause 5 (Leadership) болон 6 (Planning)-ийн нотолгоо.

---

## 3. ISMS Scope тодорхойлох үе

ISMS-ийн **scope** (хамрах хүрээ) нь бүхэл төслийн "хил хязгаар". Хэт өргөн авбал хэрэгжүүлэхэд хэцүү, хэт нарийн авбал харилцагчид итгэхгүй. CloudNova энд маш болгоомжтой хандсан.

### Scope тодорхойлох логик

Сараа дараах асуултуудаар scope-ыг тодорхойлсон:

1. Бид ямар бүтээгдэхүүн/үйлчилгээг хамгаалах гэж байна вэ? → **CloudNova SaaS платформ ба түүний дэд бүтэц**.
2. Аль хэлтэс оролцох вэ? → Engineering, IT, Security, Product, Customer Success (data хүрдэг бүх хэлтэс).
3. Аль байршил? → Улаанбаатар төв оффис, AWS Singapore/HK region, бүх remote ажилтан.
4. Гуравдагч талуудтай интерфейс? → AWS, GitHub, M365, банкны API.

### Яг ямар системүүд хамрагдах вэ?

- CloudNova SaaS production орчин бүхэлдээ (EKS, RDS, S3, гэх мэт).
- CI/CD pipeline (GitHub, GitHub Actions).
- Эх кодын репозитор.
- Corporate IT (AD, M365, эндпойнт) — учир нь эдгээрээс production-д ханддаг.
- Хэрэглэгчийн дэмжлэгийн систем (Zendesk) — учир нь PII хүрдэг.

### Ямар системүүд хамрагдахгүй үлдэх вэ? (Exclusions)

- Сингапурын борлуулалтын оффисын зөвхөн маркетингийн дотоод сүлжээ (production-д хүрдэггүй, тусдаа сегмент).
- Хуучин ERP (decommission хийгдэх төлөвт, production data байхгүй).
- Marketing вэбсайт (статик, PII боловсруулдаггүй).

**Чухал:** Аливаа exclusion-ийг **үндэслэлтэй** тайлбарлах ёстой. "Энэ нь хялбар учраас хассан" гэж болохгүй. Үндэслэл нь "энэ систем нь scope доторх мэдээллийг боловсруулдаггүй, тусгаарлагдсан" байх ёстой.

### Scope document хэрхэн бичих вэ?

Scope statement богино, тодорхой байх ёстой. CloudNova-ийн жинхэнэ scope statement:

> *"Энэхүү ISMS нь CloudNova LLC-ийн HR ба цалингийн SaaS платформыг хөгжүүлэх, түгээх, дэмжих, түүнтэй холбоотой хэрэглэгчийн мэдээллийг боловсруулах үйл ажиллагааг хамарна. Үүнд CloudNova-ийн AWS (ap-southeast-1, ap-east-1) дахь production дэд бүтэц, GitHub Enterprise дэх эх код ба CI/CD, Microsoft 365 дээрх корпорэйт мэдээллийн систем, Улаанбаатар дахь төв оффис ба алсын ажилтнуудын хандалт багтана. Statement of Applicability (SoA) хувилбар 1.0-д заасан Annex A хяналтуудыг хэрэгжүүлнэ."*

> **Бодит артефакт:** "ISMS Scope Statement v1.0" (PDF, CEO гарын үсэгтэй), Confluence хуудас, scope-ийн хил хязгаарыг харуулсан network diagram. Аудитор Stage 1-д хамгийн түрүүнд энэ баримтыг шалгана.

---

## 4. Asset Inventory боловсруулах үе

"Юу эзэмшиж байгаагаа мэдэхгүй бол хамгаалж чадахгүй." Asset inventory нь risk assessment-ийн суурь. CloudNova энд **2 долоо хоног** зарцуулсан.

### Ямар хөрөнгүүд бүртгэх вэ?

ISO 27001-д asset гэдэгт зөвхөн төхөөрөмж биш, дараах бүгд багтана:

- **Information assets:** Хэрэглэгчийн PII, цалингийн өгөгдөл, эх код, тохиргооны нууц.
- **Software assets:** SaaS аппликейшнүүд, дотоод хэрэгсэл.
- **Physical assets:** Сервер, лаптоп, сүлжээний төхөөрөмж.
- **Cloud assets:** EC2, RDS, S3 bucket, IAM role.
- **Services:** AWS, GitHub, M365.
- **People/roles:** Critical knowledge эзэмшигч (key person risk).

### Серверүүд

Тэмүүлэн AWS-ээс автоматаар хөрөнгийг цуглуулсан. Гараар бичих биш — **AWS Config + Resource Explorer** ашиглан бүх EC2, RDS, S3, Lambda-г экспортолсон. Дотоод серверийг VMware vCenter-ээс гаргасан.

### Database

- RDS PostgreSQL (prod-main) — **Confidentiality: High, Integrity: High, Availability: High** — учир нь бүх хэрэглэгчийн PII энд байна.
- RDS (analytics-replica) — High/Medium/Medium.
- DynamoDB (session store) — Medium.

### SaaS үйлчилгээ

Salesforce, Zendesk, BambooHR, Slack, Jira гэх мэт. Тус бүрд "ямар өгөгдөл агуулдаг, хэн эзэмшдэг, ямар classification-тай" гэдгийг тэмдэглэсэн.

### Ажилтны төхөөрөмжүүд

Intune/Jamf-аас 300 эндпойнтын жагсаалт автоматаар татагдсан — серийн дугаар, эзэмшигч, OS хувилбар, encryption статус, EDR статус.

### API key, Secrets

Энэ нь ихэвчлэн мартагддаг боловч хамгийн эмзэг хөрөнгө. Тэмүүлэн **HashiCorp Vault болон AWS Secrets Manager**-ийн бүх нууцыг тоолж, "хэн ямар нууц эзэмшдэг, хэзээ rotate хийсэн" гэдгийг бүртгэсэн. GitHub дээр **secret scanning** асааж, кодод хатуу бичигдсэн нууц байгаа эсэхийг шалгасан (хэдийг олж, нэн даруй устгасан).

### Source code repository / GitHub

GitHub Enterprise дээрх бүх репозиторыг ангилсан: "production code (critical)", "internal tools (medium)", "archived (low)". Хэн admin эрхтэй, ямар репо public/private гэдгийг бүртгэсэн.

### CI/CD системүүд

GitHub Actions workflow-ууд, тэдгээрийн OIDC role, runner, ямар secret-д хандах эрхтэй — бүгд бүртгэгдсэн. CI/CD нь "production-д код түлхдэг" учир маш критик хөрөнгө.

### Asset register хэрхэн үүсгэх вэ?

CloudNova эхэндээ Google Sheets дээр **Asset Register** хийсэн (хожим ISMS платформ руу шилжүүлсэн). Багана бүр:

| Asset ID | Asset нэр | Төрөл | Эзэмшигч | Хадгалах байршил | Classification (C/I/A) | Холбоотой эрсдэл |
|---|---|---|---|---|---|---|
| AST-001 | RDS prod-main (PII) | Database | Тэмүүлэн | AWS ap-southeast-1 | H/H/H | RSK-014, RSK-022 |
| AST-002 | GitHub prod repo | Source code | Тэмүүлэн | GitHub Enterprise | H/H/M | RSK-031 |
| AST-003 | Employee laptops | Endpoint | Батбаяр | Хувьсах | M/M/M | RSK-007 |
| AST-004 | M365 / Entra ID | Identity | Батбаяр | Microsoft Cloud | H/H/H | RSK-002 |
| AST-005 | AWS root account | Cloud identity | Энхбат | AWS | H/H/H | RSK-001 |

**Гол зарчим:** Хөрөнгө бүрт **эзэмшигч (owner)** заавал байх ёстой. "Эзэмшигчгүй хөрөнгө" гэдэг нь "хамгаалагдаагүй хөрөнгө". Эзэмшигч нь тухайн хөрөнгийн classification, хандалт, эрсдэлийг хариуцна.

> **Бодит артефакт:** Asset Register (эхэндээ Sheets, дараа нь ISMS tool), AWS Config snapshot, Intune device export, GitHub repo list. Аудитор "Энэ хөрөнгийн эзэмшигч хэн бэ? Хэзээ хамгийн сүүлд шинэчилсэн бэ?" гэж асууна — тиймээс register-ийг **тогтмол шинэчилж**, шинэчилсэн огноог тэмдэглэх нь чухал.

---

## 5. Risk Assessment хийх үе

Энэ нь ISO 27001-ийн **зүрх**. Бүх хяналт (control) нь эрсдэлээс үүдэлтэй байх ёстой — "ISO хэлсэн учраас" биш, "энэ эрсдэлийг бууруулахын тулд" гэсэн логиктой.

### Эрсдэл хэрхэн илрүүлэх вэ?

CloudNova **asset-based + scenario-based** холимог аргыг ашигласан. Сараа дараах эх сурвалжаас эрсдэл цуглуулсан:

- Asset register дэх хөрөнгө бүр (хөрөнгө → ямар threat-д өртөх вэ?).
- Workshop — Тэмүүлэн, Болор, Батбаяр нартай "хамгийн муу тохиолдол" brainstorm.
- Өмнөх incident-ууд (Jira, Zendesk-ээс).
- Threat intelligence (MITRE ATT&CK, OWASP Top 10, cloud threat reports).
- Vulnerability scanner-ийн үр дүн.

### Risk-ийн бүрэлдэхүүн хэсэг

ISO 27001-д эрсдэлийг дараах байдлаар бүрдүүлдэг:

- **Asset** — юу эрсдэлд байгаа вэ? (жишээ: RDS дэх PII)
- **Threat** — ямар аюул? (жишээ: халдагч өгөгдөл хулгайлах)
- **Vulnerability** — ямар сул тал ашиглах вэ? (жишээ: сул нууц үг, нээлттэй порт)
- **Impact** — болвол хэр хохирол? (1-5)
- **Likelihood** — болох магадлал? (1-5)
- **Risk score** = Impact × Likelihood

### Risk matrix (5×5)

CloudNova 5×5 матриц ашигласан:

| | L=1 | L=2 | L=3 | L=4 | L=5 |
|---|---|---|---|---|---|
| **I=5** | 5 | 10 | 15 | 20 | 25 |
| **I=4** | 4 | 8 | 12 | 16 | 20 |
| **I=3** | 3 | 6 | 9 | 12 | 15 |
| **I=2** | 2 | 4 | 6 | 8 | 10 |
| **I=1** | 1 | 2 | 3 | 4 | 5 |

- **1-5 (ногоон):** Low — хүлээн зөвшөөрч болно.
- **6-12 (шар):** Medium — төлөвлөгөөт хугацаанд эмчилнэ.
- **15-25 (улаан):** High — нэн даруй эмчилнэ.

**Risk acceptance criteria** (Clause 6.1.2-ийн шаардлага): "8-аас доош оноотой эрсдэлийг CISO хүлээн зөвшөөрч болно; 8-аас дээш бол Steering Committee шийднэ." Энэ босгыг **бичгээр баримтжуулах** ёстой.

### Нэг жишээ эрсдэлийг эхнээс нь дуустал бүрэн задлая

**RSK-014: RDS дэх хэрэглэгчийн PII задрах эрсдэл**

- **Asset:** AST-001 — RDS prod-main (бүх хэрэглэгчийн цалин, банкны данс, регистр).
- **Threat:** Гадны халдагч эсвэл дотоод хортой ажилтан өгөгдлийн санд зөвшөөрөлгүй хандаж, PII хулгайлах / задруулах.
- **Vulnerability (одоогийн байдлаар):**
  1. DB-д шууд хандах эрхтэй DevOps хүн олон (least privilege алдагдсан).
  2. DB credential нь хэдэн микросервисд хатуу бичигдсэн.
  3. DB-рүү хандалтын лог Splunk-д бүрэн ороогүй.
  4. Encryption at rest идэвхтэй боловч хандалтын аудит сул.
- **Impact:** 5 (400,000 хүний PII задрах нь зохицуулагчийн торгууль, харилцагч алдах, нэр хүндийн хохирол).
- **Likelihood (одоогийн):** 4 (сул тал олон, ийм халдлага түгээмэл).
- **Inherent risk score:** 5 × 4 = **20 (High, улаан).**

Энэ нь "**inherent risk**" буюу одоо байгаа хяналтыг тооцолгүй гаргасан түвшин. Дараагийн хэсэгт (Risk Treatment) бид үүнийг хэрхэн **residual risk** болгож бууруулсныг үзнэ.

### Risk register

Бүх эрсдэлийг **Risk Register**-т бүртгэдэг. Жишээ:

| Risk ID | Тайлбар | Asset | Impact | Likelihood | Inherent score | Эзэмшигч | Treatment | Residual score | Статус |
|---|---|---|---|---|---|---|---|---|---|
| RSK-001 | AWS root данс эвдрэх | AWS root | 5 | 3 | 15 | CISO | Mitigate (MFA, lock) | 4 | Treated |
| RSK-002 | Entra ID admin данс эвдрэх | M365 | 5 | 3 | 15 | IT | Mitigate (PAM, MFA) | 5 | Treated |
| RSK-014 | RDS PII задрах | RDS | 5 | 4 | 20 | DevOps | Mitigate | 6 | In progress |
| RSK-022 | S3 bucket нийтэд нээлттэй болох | S3 | 5 | 3 | 15 | DevOps | Mitigate (SCP, Config) | 4 | Treated |
| RSK-031 | CI/CD pipeline эвдэрч хортой код орох | GitHub | 5 | 2 | 10 | DevOps | Mitigate | 4 | Treated |

> **Бодит артефакт:** Risk Assessment Methodology document (арга зүйг тайлбарласан), Risk Register (live spreadsheet эсвэл ISMS tool), Risk Assessment Report. Аудитор "Энэ эрсдэлийн оноог яаж гаргасан бэ? Методологитой нь нийцэж байна уу?" гэж асууна — иймээс **методологи нь register-тэй тохирч** байх ёстой.

---

## 6. Risk Treatment хийх үе

Эрсдэл бүрд дөрвөн сонголтын аль нэгийг сонгоно. Үүнийг **Risk Treatment Plan (RTP)**-д бичнэ.

### 1. Бууруулах (Mitigate / Modify)

Хамгийн түгээмэл сонголт — хяналт нэмж эрсдэлийг бууруулна.

**Жишээ — RSK-014 (RDS PII):** CloudNova дараах хяналтуудыг хэрэгжүүлсэн:
- DB-д шууд хандах эрхийг устгаж, зөвхөн **bastion + just-in-time access** (Teleport)-аар хандах болгосон.
- DB credential-ийг кодоос гаргаж, **Vault dynamic secrets** болгосон (богино настай).
- Бүх DB query-г **CloudTrail + RDS audit log → Splunk** руу дамжуулж, мониторинг тохируулсан.
- DB-рүү хандах хүний тоог 8-аас 2 болгосон (least privilege).

Үр дүн: Likelihood 4 → 1.5 ≈ 2 болж буурсан. **Residual score = 5 × 2 ≈ 6 (шар, хүлээн зөвшөөрөгдөх).** Энэ нь treatment-ийн жинхэнэ үр дүн — эрсдэлийг тэглээгүй, харин хүлээн зөвшөөрөх түвшинд хүртэл бууруулсан.

### 2. Шилжүүлэх (Transfer / Share)

Эрсдэлийг гуравдагч тал руу шилжүүлнэ.

**Жишээ:** Cyber insurance худалдан авсан (дата задралын зардлыг хэсэгчлэн нөхөх). Эсвэл AWS-ийн **shared responsibility model**-аар дэд бүтцийн физик аюулгүй байдлыг AWS хариуцдаг. Гэхдээ "шилжүүлэх" нь хариуцлагыг бүрэн арилгахгүй — нэр хүндийн хохирлыг даатгал нөхөхгүй.

### 3. Хүлээн зөвшөөрөх (Accept / Retain)

Эрсдэл бага, эсвэл эмчлэх зардал нь хохирлоос их бол хүлээн зөвшөөрнө.

**Жишээ:** "Маркетингийн статик вэбсайт DDoS халдлагад өртөх" эрсдэл (score 4, low). CloudFront хамгаалалт хангалттай, нэмэлт хөрөнгө оруулалт зохисгүй гэж үзээд CISO бичгээр хүлээн зөвшөөрсөн. **Хүлээн зөвшөөрөл нь үргэлж эрх мэдэлтэй хүний гарын үсэгтэй байх ёстой.**

### 4. Зайлсхийх (Avoid / Eliminate)

Эрсдэл үүсгэж буй үйл ажиллагааг бүхэлд нь зогсооно.

**Жишээ:** Хуучин ERP нь патч хийгдэхгүй, эмзэг байсан. Үүнийг засахын оронд **бүхэлд нь decommission** хийж, шинэ SaaS руу шилжсэн. Эрсдэлийг "эмчлэх" биш "арилгасан".

### Statement of Applicability (SoA) — хамгийн чухал баримт

Risk treatment-ийн үр дүнд **SoA** гарна. Энэ нь Annex A-ийн **93 хяналт** (ISO 27001:2022) тус бүрд:
- Хэрэгжүүлсэн үү, үгүй юу?
- Хэрэв тийм бол яаж?
- Хэрэв үгүй бол яагаад? (justification for exclusion)

SoA нь аудиторын хамгийн их шалгадаг баримтуудын нэг. CloudNova-ийн SoA жишээ хэсэг:

| Control | Нэр | Applicable? | Хэрэгжилт | Нотолгоо |
|---|---|---|---|---|
| A.5.15 | Access control | Тийм | IAM, RBAC, MFA | IAM policy, MFA report |
| A.8.7 | Protection against malware | Тийм | CrowdStrike EDR | EDR dashboard |
| A.8.16 | Monitoring activities | Тийм | Splunk SIEM | SIEM alert rules |
| A.7.4 | Physical security monitoring | Тийм (оффис) | CCTV, card access | Access log |
| A.5.7 | Threat intelligence | Тийм | GuardDuty, feeds | TI report |

> **Бодит артефакт:** Risk Treatment Plan (эрсдэл бүрд action, эзэмшигч, deadline), Statement of Applicability v1.0, Risk Acceptance form (гарын үсэгтэй). Jira дээр treatment бүр нэг task болж, эзэмшигч, дуусгах хугацаатай явдаг.

---

## 7. Annex A Controls хэрэгжүүлэх үе

ISO 27001:2022-ийн Annex A нь **93 хяналт**, 4 ангилалд: Organizational (37), People (8), Physical (14), Technological (34). Энд хамгийн чухал хяналтуудыг **яагаад → хэн → ямар хэрэгсэл → ямар баримт → ямар нотолгоо** гэсэн бүтцээр задлая.

### Access Control (A.5.15, A.5.18, A.8.2, A.8.3)

- **Яагаад хэрэгтэй:** Зөвхөн зөвшөөрөгдсөн хүн зөвхөн хэрэгтэй нөөцдөө хандах ёстой (least privilege). PII-д хандах эрх хяналтгүй бол дата задрах эрсдэл өсдөг.
- **Хэн хэрэгжүүлдэг:** IT (Батбаяр) — corporate; DevOps (Тэмүүлэн) — cloud/production.
- **Хэрэгсэл:** Entra ID (RBAC, Conditional Access), AWS IAM + IAM Identity Center, GitHub teams, RBAC in app.
- **Баримт:** Access Control Policy, Joiner-Mover-Leaver (JML) procedure, access review records.
- **Нотолгоо:** Улирал бүрийн **access review** (хэн ямар эрхтэй вэ гэдгийг эзэмшигч баталгаажуулсан), terminated user-ийн данс хаасан лог, privileged эрхийн жагсаалт.

CloudNova улирал бүр **User Access Review** хийдэг: эзэмшигч бүр "миний хариуцсан системийн хандалтын жагсаалт" авч, шаардлагагүйг устгана. Энэ нь Jira ticket болж явдаг, баталгаажсан screenshot нь нотолгоо.

### MFA (A.8.5 — Secure authentication)

- **Яагаад:** Зөвхөн нууц үг хангалтгүй; phishing, credential stuffing-ийн эсрэг хамгийн үр дүнтэй хяналт.
- **Хэн:** IT (M365), DevOps (AWS), бүх SaaS admin.
- **Хэрэгсэл:** Entra ID MFA / FIDO2 security keys (admin-д), AWS MFA, GitHub 2FA enforced.
- **Баримт:** MFA policy, Conditional Access policy.
- **Нотолгоо:** Entra ID MFA registration report (100% хамрагдалт), AWS Credential Report (root + admin MFA on), GitHub org "require 2FA" setting screenshot.

CloudNova-д **admin данс бүр FIDO2 hardware key** ашигладаг (phishing-resistant). Энгийн хэрэглэгч authenticator app.

### Privileged Access Management — PAM (A.8.2)

- **Яагаад:** Admin/root эрх нь "крон үнэт зүйл". Эдгээр эвдэрвэл бүх систем эвдэрнэ.
- **Хэн:** IT + DevOps + CISO.
- **Хэрэгсэл:** CyberArk / Teleport (just-in-time access), AWS IAM Identity Center permission sets, AWS root data lock.
- **Баримт:** Privileged Access Policy.
- **Нотолгоо:** JIT access log (хэн, хэзээ, ямар эрх, хэр удаан), session recording, эрх олгох approval workflow.

CloudNova-д production-д **standing admin эрх байхгүй**. Хэрэв инженер RDS-д хандах бол Teleport-оор хүсэлт гаргаж, 4 нүдний зарчмаар (Тэмүүлэн эсвэл CISO) баталж, 2 цагийн хугацаатай эрх авна. Бүх session бичигдэнэ.

### Logging (A.8.15)

- **Яагаад:** Юу болсныг мэдэхгүй бол хариу үйлдэл хийж чадахгүй; incident-ийг шинжлэх, forensic хийхэд лог зайлшгүй.
- **Хэн:** DevOps (cloud log), IT (endpoint/AD log), SOC (нэгтгэх).
- **Хэрэгсэл:** AWS CloudTrail, VPC Flow Logs, RDS audit, M365 audit, CrowdStrike, бүгд → Splunk.
- **Баримт:** Logging & Monitoring Policy.
- **Нотолгоо:** Splunk-д лог ирж байгааг харуулсан dashboard, log retention setting (1 жил+), log integrity (immutable S3 bucket, object lock).

**Чухал:** Лог нь **өөрчлөгдөшгүй** байх ёстой. CloudNova лог-уудыг S3 Object Lock бүхий bucket-д хадгалдаг — admin ч устгаж чадахгүй.

### Monitoring (A.8.16)

- **Яагаад:** Лог цуглуулаад харахгүй бол утгагүй. Аномали, халдлагыг бодит цагт илрүүлэх.
- **Хэн:** SOC (Болор).
- **Хэрэгсэл:** Splunk correlation rules, GuardDuty, Security Hub, Datadog.
- **Баримт:** Monitoring procedure, alert response runbook.
- **Нотолгоо:** Alert rule definitions, triggered alert-ийн жишээ, тэдгээрийг шийдвэрлэсэн тикет.

Болор өдөр бүр өглөө **SOC dashboard** шалгана: шөнийн alert-ууд, GuardDuty findings, амжилтгүй нэвтрэлтийн оролдлогууд. Сэжигтэй зүйл олвол incident ticket нээнэ.

### Vulnerability Management (A.8.8)

- **Яагаад:** Засагдаагүй сул тал нь халдагчийн орох хаалга.
- **Хэн:** DevOps (дэд бүтэц), Engineering (код), Security.
- **Хэрэгсэл:** AWS Inspector, Trivy (container scan), Snyk/Dependabot (dependency), Nessus (зарим).
- **Баримт:** Vulnerability Management Policy (SLA-тай: Critical 7 хоног, High 30 хоног).
- **Нотолгоо:** Scan reports, vulnerability-ийг хаасан Jira ticket, SLA нийцлийн тайлан.

CloudNova долоо хоног бүр scan хийж, critical/high vuln-ийг Jira-д автоматаар ticket болгож, SLA-тай эмчилнэ.

### Patch Management (A.8.8 холбоотой)

- **Яагаад:** Vuln-ийг хаах гол арга.
- **Хэн:** IT (endpoint/OS), DevOps (server/container).
- **Хэрэгсэл:** Intune (Windows), Jamf (Mac), automated container rebuild (immutable infra).
- **Баримт:** Patch Management Procedure.
- **Нотолгоо:** Patch compliance report (хэдэн % эндпойнт up-to-date), patch deployment лог.

CloudNova-ийн container-ууд **immutable** — патч хийхгүй, харин шинэ base image-аар дахин build хийж deploy хийдэг. Энэ нь "patch drift"-ийг арилгана.

### Backup (A.8.13)

- **Яагаад:** Ransomware, алдаатай устгал, дэд бүтцийн эвдрэлээс сэргээх.
- **Хэн:** DevOps.
- **Хэрэгсэл:** RDS automated snapshots, S3 cross-region replication, AWS Backup.
- **Баримт:** Backup Policy (3-2-1 зарчим: 3 хувь, 2 төрлийн медиа, 1 offsite).
- **Нотолгоо:** Backup success log, **restore test-ийн нотолгоо** (хамгийн чухал — нөөцлөлт хийгээд сэргээж чаддаггүй бол утгагүй).

CloudNova улирал бүр **restore test** хийдэг: RDS snapshot-аас тусдаа орчинд сэргээж, өгөгдөл бүрэн эсэхийг шалгаж, тэмдэглэл үлдээнэ.

### Incident Response (A.5.24-A.5.28)

- **Яагаад:** Incident гарах нь "болох уу үгүй юу" биш "хэзээ" гэдэг асуудал. Бэлэн байх нь хохирлыг бууруулна.
- **Хэн:** SOC (Болор), CISO, бүх баг (severity-ээс хамаарч).
- **Хэрэгсэл:** PagerDuty (alerting), Jira (tracking), Slack war room, Splunk (investigation).
- **Баримт:** Incident Response Plan, severity classification, runbook-ууд.
- **Нотолгоо:** Incident tickets, post-mortem reports, **table-top exercise** тэмдэглэл.

CloudNova жилд 2 удаа **incident simulation (table-top)** хийдэг: "S3 bucket нийтэд нээлттэй болж PII задарлаа" гэсэн хувилбараар баг хариу үйлдлээ дадлагажуулна. Энэ нь IR plan-ийн нотолгоо.

### Business Continuity (A.5.29, A.5.30)

- **Яагаад:** Том саатал (region outage, гамшиг) гарвал бизнес үргэлжлэх ёстой.
- **Хэн:** CISO, DevOps, удирдлага.
- **Хэрэгсэл:** Multi-AZ, multi-region failover, DR runbook.
- **Баримт:** Business Continuity Plan (BCP), Disaster Recovery Plan (DRP), RTO/RPO тодорхойлсон.
- **Нотолгоо:** BCP document, **DR test** тэмдэглэл (жинхэнэ failover дадлага).

CloudNova-ийн **RTO = 4 цаг, RPO = 15 минут**. Жилд нэг удаа secondary region руу failover дадлага хийнэ.

### Supplier Security (A.5.19-A.5.23)

- **Яагаад:** Нийлүүлэгчийн эвдрэл нь таны эвдрэл (supply chain risk). AWS, GitHub, M365 бүгд эрсдэл агуулна.
- **Хэн:** Хууль (Мөнхзул) + Security.
- **Хэрэгсэл:** Vendor risk questionnaire, нийлүүлэгчийн SOC 2 / ISO 27001 сертификат цуглуулах.
- **Баримт:** Supplier Security Policy, vendor register, DPA (Data Processing Agreement).
- **Нотолгоо:** Vendor risk assessment бүрд, гэрээний security clause, нийлүүлэгчийн compliance сертификат.

CloudNova критик нийлүүлэгч бүрийн (AWS, GitHub гэх мэт) ISO 27001/SOC 2 тайланг жил бүр цуглуулж, эрсдэлийг үнэлдэг.

### Secure Development (A.8.25-A.8.28)

- **Яагаад:** Эмзэг код нь хамгийн том халдлагын гадаргуу. Secure SDLC шаардлагатай.
- **Хэн:** Engineering, DevOps, Security (champions).
- **Хэрэгсэл:** SAST (Snyk Code/SonarQube), DAST, dependency scan (Dependabot), code review, secret scanning.
- **Баримт:** Secure Development Policy, coding standards, code review checklist.
- **Нотолгоо:** PR-д заавал review шаардсан branch protection, scan-ийн үр дүн, security training бүртгэл.

CloudNova-д бүх PR заавал **2 reviewer + автомат security scan** дамжихгүйгээр merge хийгдэхгүй (GitHub branch protection).

### Change Management (A.8.32)

- **Яагаад:** Хяналтгүй өөрчлөлт нь outage болон security gap үүсгэдэг.
- **Хэн:** Engineering, DevOps.
- **Хэрэгсэл:** GitHub PR workflow, CI/CD gates, change ticket (Jira).
- **Баримт:** Change Management Procedure.
- **Нотолгоо:** Change tickets, approval records, deployment лог, rollback plan.

Production-д орох өөрчлөлт бүр PR + approval + автомат тест + deployment record-той. Аудитор санамсаргүй нэг deployment сонгож "энэ зөвшөөрөгдсөн үү?" гэж асууж болно.

### Asset Management (A.5.9-A.5.11)

- **Яагаад:** Хөрөнгийг бүртгэж, classification хийж, амьдралын мөчлөгийг (худалдан авах → ашиглах → устгах) удирдах.
- **Хэн:** IT, DevOps, asset owner-ууд.
- **Хэрэгсэл:** Asset register, Intune/Jamf, AWS Config.
- **Баримт:** Asset Management Policy, Acceptable Use Policy, classification scheme.
- **Нотолгоо:** Up-to-date asset register, secure disposal records (хатуу диск устгасан гэрчилгээ).

### Cryptography (A.8.24)

- **Яагаад:** Өгөгдлийг дамжуулах болон хадгалах үед хамгаалах.
- **Хэн:** DevOps, Engineering.
- **Хэрэгсэл:** TLS 1.2+/1.3 (in transit), AWS KMS (at rest), Vault (key mgmt).
- **Баримт:** Cryptography Policy (зөвшөөрөгдсөн алгоритм, түлхүүрийн урт, key rotation).
- **Нотолгоо:** TLS config (SSL Labs тест A+), KMS key policy, encryption status report (RDS/S3 encrypted).

CloudNova-д бүх PII **at rest (KMS) болон in transit (TLS 1.3)** шифрлэгдэнэ. Key rotation автоматаар жилд нэг.

> **Бодит артефакт (бүх control-д хамаатай):** Control тус бүрд нэг "control owner", нэг policy/procedure, тогтмол үүсэх нотолгоо (лог, report, screenshot, ticket). Аудитор control бүрт "Үүнийг хэрэгжүүлсэн гэдгийг батал" гэх тул **нотолгоо тогтмол үүсдэг систем** байгуулах нь зорилго — гар аргаар "аудитын өмнө цуглуулъя" гэвэл унана.


---

## 8. Policy болон Procedure боловсруулах үе

Policy нь "юу хийх ёстой", Procedure нь "яаж хийх вэ" гэдгийг тодорхойлно. Аудитор баримтыг харахдаа: (1) батлагдсан уу (огноо, эзэн, гарын үсэг), (2) хэрэгжиж байна уу (нотолгоо), (3) хянагдаж байна уу (review огноо) гэдгийг шалгана.

Сараа бүх баримтад нэгдсэн **document control** толгой хэсэг тавьсан:

```
Баримтын нэр: Access Control Policy
Хувилбар: 1.2
Эзэмшигч: CISO
Баталсан: CEO (огноо: 2026-04-15)
Сүүлийн хяналт: 2026-04-10
Дараагийн хяналт: 2027-04-10
Ангилал: Internal
```

### Information Security Policy (тэргүүлэх баримт)

Энэ нь ISMS-ийн "үндсэн хууль". Дээд түвшний, удирдлагын амлалтыг илэрхийлдэг. Жишээ хэсэг:

> *"CloudNova LLC нь харилцагч болон ажилтнуудынхаа мэдээллийн нууцлал, бүрэн бүтэн байдал, хүртээмжийг хамгаалахаа үүрэг болгоно. Бид ISO/IEC 27001 стандартад нийцсэн ISMS-ийг байгуулж, тасралтгүй сайжруулна. Мэдээллийн аюулгүй байдал нь бүх ажилтны хариуцлага бөгөөд энэ бодлогыг зөрчсөн тохиолдолд сахилгын арга хэмжээ авна. Удирдлага нь шаардлагатай нөөц, дэмжлэгийг хангана."*

Энэ баримтыг **CEO гарын үсгээр баталж**, бүх ажилтанд танилцуулна (Clause 5.2).

### Access Control Policy

Жишээ хэсэг:

> *"1. Бүх хандалт least privilege зарчмаар олгогдоно. 2. Хэрэглэгч бүр өвөрмөц данстай байх ба нийтийн (shared) данс хориглоно. 3. Privileged эрхэд just-in-time хандалт ба MFA заавал шаардана. 4. Хандалтын эрхийг улирал бүр эзэмшигч хянана. 5. Ажлаас гарсан ажилтны бүх хандалтыг 24 цагийн дотор хаана."*

### Password Policy

Орчин үеийн (NIST 800-63B-д нийцсэн) бодлого:

> *"1. Нууц үг хамгийн багадаа 14 тэмдэгт. 2. Тогтмол хугацаагаар албадан солих шаардлагагүй (зөвхөн эвдрэл сэжиглэвэл). 3. Нийтлэг/задарсан нууц үгийг хориглоно (breached password check). 4. MFA заавал. 5. Боломжтой газар passwordless (FIDO2) сонголтыг дэмжинэ."*

(Анхаар: хуучин "90 хоног бүр солих" дүрэм одоо зөвлөмж болдоггүй — энэ нь сул нууц үг рүү хөтөлдөг.)

### Incident Response Procedure

Алхам алхмаар:

> *"1. Илрүүлэх (Detect): Alert эсвэл мэдээллийн дагуу SOC ticket нээнэ. 2. Ангилах (Triage): Severity (P1-P4) тогтооно. 3. Хязгаарлах (Contain): Эвдэрсэн данс/хост-ийг тусгаарлана. 4. Устгах (Eradicate): Шалтгааныг арилгана. 5. Сэргээх (Recover): Системийг хэвийн болгоно. 6. Дүгнэх (Lessons learned): 5 ажлын өдрийн дотор post-mortem. 7. Шаардлагатай бол зохицуулагч/харилцагчид 72 цагийн дотор мэдэгдэнэ (GDPR)."*

### Backup Procedure

> *"1. RDS — өдөр бүр автомат snapshot, 35 хоног хадгална. 2. S3 — cross-region replication. 3. Нөөцлөлт KMS-ээр шифрлэгдэнэ. 4. Улирал бүр restore test хийж, тэмдэглэл үлдээнэ. 5. Restore test амжилтгүй бол P2 incident болгоно."*

### Change Management Procedure

> *"1. Бүх production өөрчлөлт PR-аар орно. 2. Хамгийн багадаа нэг (критик бол хоёр) reviewer батална. 3. Автомат тест, security scan дамжина. 4. Deployment бүр rollback plan-тай. 5. Яаралтай (emergency) өөрчлөлтийг дараа нь буцаан баримтжуулна (retroactive approval)."*

> **Бодит артефакт:** Confluence дээр "ISMS Document Library" — бүх policy/procedure нэг дор, хувилбар хяналттай. Ажилтан бүр уншиж, "acknowledged" дарсан бүртгэлтэй (M365/LMS). Энэ acknowledgment нь Clause 7.3 (Awareness)-ийн нотолгоо.

---

## 9. Техникийн хэрэгжилт

Энэ нь баг бодит хэрэгслүүдийг **яг юу тохируулдгийг** алхам алхмаар харуулна. Энэ хэсэг нь policy-г бодит config болгож хувиргадаг.

### Active Directory / Entra ID (Батбаяр)

Батбаяр дараах зүйлийг тохируулсан:
- **Tiered admin model:** Tier 0 (domain admin), Tier 1 (server admin), Tier 2 (workstation admin) тусгаарлаж, эрх холилдохоос сэргийлсэн.
- **Conditional Access:** "Зөвхөн compliant device-ээс, зөвшөөрөгдсөн байршлаас, MFA-тай нэвтрэх". Risk-based — сэжигтэй нэвтрэлтийг блоклоно.
- **Privileged Identity Management (PIM):** Admin эрх "always on" биш, хүсэлтээр идэвхждэг (just-in-time).
- **Password protection:** Banned password list, smart lockout.
- **Legacy authentication blocked:** Хуучин протокол (basic auth) бүрэн хаасан — phishing-ийн гол суваг.

### Microsoft 365 (Батбаяр)

- **MFA enforced** бүх хэрэглэгчид.
- **DLP (Data Loss Prevention):** Регистрийн дугаар, банкны данс зэрэг pattern бүхий имэйл гадагшаа явахыг хязгаарласан.
- **Safe Links / Safe Attachments (Defender for O365):** Phishing/malware-аас хамгаалах.
- **Audit log** идэвхжүүлж, Splunk руу дамжуулсан.
- **Retention & legal hold** тохируулсан.

### AWS IAM (Тэмүүлэн)

- **AWS Organizations + Control Tower:** Олон данс (prod, staging, dev, security, logging). Production data dev-ээс бүрэн тусгаарлагдсан.
- **Service Control Policies (SCP):** "S3 bucket-ийг public хийхийг хориглох", "тодорхой region-оос гадуур resource үүсгэхийг хориглох", "root данс ашиглахыг блоклох" гэх мэт organization-wide guardrails.
- **IAM Identity Center (SSO):** Хүний хэрэглэгчид IAM user биш, SSO-оор богино настай role авна. Standing credential байхгүй.
- **OIDC for CI/CD:** GitHub Actions нь хадгалсан AWS key биш, OIDC-аар богино настай эрх авна — secret leak-ийн эрсдэлийг арилгана.
- **least privilege roles:** IAM Access Analyzer-аар хэт өргөн эрхийг илрүүлж нарийсгасан.

### VPN / Zero Trust (Батбаяр + Тэмүүлэн)

- Алсын ажилтнууд **Tailscale (WireGuard)** дээр, device posture check-тэй (EDR ажиллаж байгаа, диск шифрлэгдсэн эсэхийг шалгана).
- Production-д шууд интернетээс хандах хаалга байхгүй — зөвхөн bastion/Teleport-оор.

### Firewall (Батбаяр + Тэмүүлэн)

- Оффис: Meraki MX, VLAN segmentation, IDS/IPS идэвхтэй.
- Cloud: AWS Security Groups (deny-by-default), NACL, **AWS WAF** (OWASP rules, rate limiting, geo-block).
- Бүх firewall дүрмийг **тогтмол хянаж**, ашиглагдаагүй дүрмийг устгана.

### SIEM — Splunk (Болор)

- **Data sources:** CloudTrail, VPC Flow, GuardDuty, M365 audit, CrowdStrike, RDS audit, app logs.
- **Correlation rules (жишээ):**
  - "Богино хугацаанд олон амжилтгүй нэвтрэлт → brute force alert"
  - "IAM policy өөрчлөгдсөн → admin-д alert"
  - "S3 bucket policy public болсон → P1 alert"
  - "Шинэ IAM user үүссэн → review alert"
  - "Impossible travel (2 өөр улсаас 10 минутын зайтай нэвтрэлт) → alert"
- **Dashboard-ууд:** SOC өдөр тутмын dashboard, executive security dashboard.

### EDR — CrowdStrike Falcon (Болор + Батбаяр)

- Бүх эндпойнт (Windows, Mac) болон production server-д суулгасан.
- **Real-time detection & response:** Хортой процесс илрэхэд автоматаар containment.
- **USB control:** Зөвшөөрөлгүй USB хадгалах төхөөрөмжийг блоклосон.
- Findings → Splunk → SOC workflow.

### Endpoint Security (Батбаяр)

- **Intune (Windows) / Jamf (Mac):** BitLocker/FileVault disk encryption албадсан, screen lock policy, patch baseline, compliant биш бол Conditional Access-аар корпорэйт нөөцөөс блоклоно.
- **Application control:** Зөвшөөрөгдсөн app-ууд.

### GitHub Security (Тэмүүлэн)

- **Branch protection:** main branch-д шууд push хориотой, PR + 2 review + status check шаардана.
- **Required 2FA** бүх гишүүнд.
- **Secret scanning + push protection:** Нууц push хийхийг бодит цагт блоклоно.
- **Dependabot:** Эмзэг dependency-г автоматаар илрүүлж PR гаргана.
- **CODEOWNERS:** Критик код өөрчлөгдвөл тухайн багийн review заавал.
- **SAST (CodeQL):** Кодын аюулгүй байдлын автомат шинжилгээ CI-д.

### Secrets Management (Тэмүүлэн)

- **HashiCorp Vault:** Динамик нууц (DB credential богино настай, ашиглах бүрд шинээр), encryption-as-a-service.
- **AWS Secrets Manager:** Автомат rotation бүхий нууцууд.
- **Зарчим:** Хүн нууцыг хэзээ ч "харахгүй" — аппликейшн ажиллах үедээ Vault-аас динамикаар авна. Кодод нууц байхгүй (secret scanning баталгаажуулна).

> **Бодит артефакт:** Config screenshot-ууд (Conditional Access policy, SCP JSON, WAF rules, branch protection settings, Splunk correlation rules), architecture diagram, "hardening checklist" (CIS Benchmark-д нийцсэн). Аудитор Stage 2-д эдгээр тохиргоог **бодитоор харна** — "MFA асаалттай гэдгийг live харуулаач" гэж асууж болно.

---

## 10. Awareness Training

Хичнээн сайн технологи байсан ч ажилтан phishing-д орвол бүгд утгагүй. Хүн бол хамгийн сул холбоос — мөн зөв сургавал хамгийн хүчтэй хамгаалалт.

### Ажилтнуудыг хэрхэн сургах вэ?

CloudNova олон давхаргат хандлага ашигласан:
- **Onboarding training:** Шинэ ажилтан орохдоо заавал security induction дамжина (data handling, password, phishing, clean desk).
- **Жил тутмын refresher:** Бүх ажилтан жилд нэг удаа (KnowBe4 / дотоод LMS дээр) дамжина.
- **Role-based training:** Хөгжүүлэгчид secure coding (OWASP), IT/DevOps-д cloud security гэх мэт тусгай.
- **Micro-learning:** Сар бүр богино (3-5 минут) видео/имэйл.

### Phishing simulation хэрхэн хийх вэ?

- CloudNova сар бүр **симуляц phishing** имэйл илгээдэг (KnowBe4 эсвэл Microsoft Attack Simulator).
- Дарсан хүнд шууд "Энэ симуляц байсан" гэсэн боловсролын хуудас гарна (ичээх биш, сургах).
- **Click rate**-ийг хэмжиж, удирдлагад тайлагнана. Зорилго: click rate-ийг улирал бүр бууруулах (жишээ: 18% → 5%).
- Дахин дахин дардаг хүнд нэмэлт сургалт.

### Сургалтын бүртгэл хэрхэн хөтлөх вэ?

Энэ нь Clause 7.2 (Competence) ба 7.3 (Awareness)-ийн нотолгоо:
- LMS дээр хэн хэзээ ямар сургалт дамжсан бүртгэл.
- 100% completion-ийг хянаж, дамжаагүй хүнд сануулга.
- Phishing simulation үр дүнгийн тайлан.
- Onboarding checklist дээр "security training дамжсан" гэсэн тэмдэг.

> **Бодит артефакт:** Training completion report (хэн, хэзээ, ямар оноо), phishing simulation report (click rate тренд), onboarding checklist, training material-ийн хувилбарууд. Аудитор "Сүүлийн сард орсон ажилтан security training дамжсан уу? Нотлоорой" гэж асууж болно.

---

## 11. Internal Audit

Сертификатын аудитын өмнө компани **өөрийгөө** аудит хийх ёстой (Clause 9.2). Энэ нь "бид бэлэн үү?" гэдгийг шалгах дотоод бэлтгэл.

### Аудит хэрхэн төлөвлөх вэ?

- **Audit programme:** Оюун жилийн аудитын төлөвлөгөө гаргасан — ямар хэсгийг хэзээ аудит хийх. Эрсдэл өндөр хэсгийг (production, access control) илүү давтамжтай.
- **Хараат бус байдал:** Оюун өөрийн хэрэгжүүлээгүй хэсгийг аудит хийнэ. DevOps-ийн хийсэн зүйлийг DevOps аудит хийж болохгүй.
- **Audit plan:** Аудит бүрд scope, criteria (ISO 27001 + CloudNova policy), хуваарь, ярилцах хүмүүс.

### Evidence хэрхэн цуглуулах вэ?

Оюун дараах байдлаар нотолгоо цуглуулна:
- **Document review:** Policy батлагдсан уу, шинэчлэгдсэн үү.
- **Configuration review:** MFA асаалттай юу (live харах), SCP идэвхтэй юу.
- **Records review:** Access review хийгдсэн үү, backup restore test-ийн тэмдэглэл байна уу.
- **Sampling:** Сүүлийн 3 сард гарсан 10 change ticket санамсаргүй сонгож, бүгд зөвшөөрөгдсөн эсэхийг шалгах.

### Interview хэрхэн хийх вэ?

Оюун ажилтнуудтай ярилцана (зэмлэх биш, баталгаажуулах):
- Болороос: "Шинэ alert ирвэл та яаж хариу үйлдэл хийдэг вэ? Сүүлийн жишээ үзүүлээч."
- Тэмүүлэнгээс: "Production-д хандах эрхийг яаж авдаг вэ?"
- Энгийн ажилтнаас: "Сэжигтэй имэйл ирвэл яадаг вэ?"

Хариулт нь policy-тэй нийцэж байгаа эсэхийг шалгана. Хэрэв ажилтан процессоо мэдэхгүй бол — энэ нь awareness gap.

### Nonconformity хэрхэн илрүүлэх вэ?

Оюун дараах төрлийн зөрчлийг бүртгэнэ:
- **Major NC:** Системийн томоохон цоорхой (жишээ: production-д MFA байхгүй).
- **Minor NC:** Жижиг алдаа (жишээ: нэг хэлтэст access review саатсан).
- **Observation / OFI:** Сайжруулах боломж (заавал биш, зөвлөмж).

Жишээ олдвол: "RSK-014-ийн treatment-д DB access review улирал бүр гэж заасан боловч сүүлийн review 5 сарын өмнө хийгдсэн" → Minor NC. Энэ нь **Corrective Action**-ийг шаардана: үндсэн шалтгааныг олж, засаж, дахин гарахаас сэргийлэх.

> **Бодит артефакт:** Internal Audit Programme, Audit Plan, Audit Report (findings, NC, OFI), Nonconformity log, Corrective Action records. Энэ нь сертификатын аудитор хамгийн их үнэлдэг — "Та өөрийгөө хянадаг систем байгуулсан уу?" гэдгийн нотолгоо.

---

## 12. Management Review

Удирдлага ISMS-ийг тогтмол (жилд хамгийн багадаа нэг, ихэвчлэн улирал бүр) хянах ёстой (Clause 9.3). Энэ нь "ISMS амьд байна уу, ажиллаж байна уу?" гэдгийг удирдлага баталгаажуулдаг.

### Удирдлагад ямар тайлан үзүүлэх вэ?

Сараа Steering Committee-д дараах багцыг бэлдэнэ (Clause 9.3-ийн заавал орох оролтууд):
- Өмнөх review-ийн action-ийн биелэлт.
- Дотоод/гадаад нөхцөл байдлын өөрчлөлт.
- ISMS гүйцэтгэлийн санал хүсэлт: NC, corrective action, monitoring үр дүн, аудитын дүн.
- Эрсдэлийн төлөв байдал, treatment-ийн явц.
- Сайжруулах боломжууд.
- Нөөцийн хэрэгцээ.

### KPI / Security metrics жишээнүүд

- **Эрсдэлийн метрик:** Нээлттэй high эрсдэлийн тоо, SLA-аас хэтэрсэн treatment.
- **Vulnerability metrics:** Critical vuln-ийн дундаж засах хугацаа (MTTR), SLA нийцэл %.
- **Incident metrics:** Сарын incident тоо, дундаж detect хугацаа (MTTD), хариу хугацаа (MTTR).
- **Access metrics:** Access review гүйцэтгэлийн %, orphaned account тоо.
- **Awareness metrics:** Training completion %, phishing click rate тренд.
- **Backup metrics:** Backup success rate, restore test үр дүн.
- **Patch metrics:** Patch compliance %.

### Management Review-ийн гаралт

- Сайжруулах шийдвэрүүд.
- Нөөц хуваарилах шийдвэр.
- Зорилгод оруулах өөрчлөлт.

> **Бодит артефакт:** Management Review meeting minutes (заавал орох оролт, гарсан шийдвэр), security metrics dashboard (Splunk/Confluence), action items log. Энэ minutes нь Clause 9.3-ийн нотолгоо — аудитор "Удирдлага сүүлд хэзээ ISMS-ийг хянасан бэ? Юу шийдсэн бэ?" гэж асууна.


---

## 13. Certification Audit

Гэрчилгээжүүлэх байгууллага (Certification Body — жишээ нь BSI, DNV, TÜV, Bureau Veritas) хоёр үе шаттай аудит хийнэ.

### Stage 1 — Documentation / Readiness Review

**Аудитор юу шалгадаг вэ?** Stage 1 нь голдуу баримтын аудит — "Та бэлэн үү?" гэдгийг шалгана:
- ISMS Scope statement зөв тодорхойлогдсон уу?
- Information Security Policy батлагдсан уу?
- Risk Assessment методологи болон Risk Register байгаа юу?
- Statement of Applicability (SoA) бүрэн уу?
- Risk Treatment Plan байгаа юу?
- Mandatory documents бүгд байгаа юу? (Scope, Policy, Risk methodology, SoA, RTP, мэдээллийн аюулгүй байдлын зорилго, Internal audit, Management review, эрх мэдэл/үүрэг).
- Internal audit нэг удаа хийгдсэн үү?
- Management review хийгдсэн үү?

**Үр дүн:** Аудитор Stage 2-т бэлэн эсэхийг дүгнэж, цоорхой (gap) заана. CloudNova-д Stage 1-д хэдэн жижиг observation гарсан (жишээ: нэг policy-д review огноо дутуу). Эдгээрийг Stage 2-оос өмнө засна.

**Stage 1-д асууж болох жишээ асуултууд:**
- "ISMS-ийн scope-ыг хэн батласан бэ?"
- "Эрсдэлийн оноог хэрхэн тооцдог методологитой нь танилцуулна уу?"
- "SoA дээр A.5.7 (threat intelligence)-ийг applicable гэсэн — яаж хэрэгжүүлсэн бэ?"

### Stage 2 — Certification Audit (бодит хэрэгжилт)

**Аудитор юу шалгадаг вэ?** Stage 2 нь "Баримт дээр бичсэн зүйл бодит амьдрал дээр ажиллаж байна уу?" гэдгийг шалгана. Аудитор нотолгоо (evidence) шаардаж, ажилтнуудтай ярилцана:
- **Хэрэгжилтийн нотолгоо:** "MFA бүх admin-д асаалттай гэдгийг live харуулаач" → Батбаяр Entra ID-аас үзүүлнэ.
- **Sampling:** "Сүүлийн улирлын access review-ийн нотолгоог үзүүлээч" → Сараа гарын үсэгтэй review records харуулна.
- **Process walkthrough:** "Танай incident response яаж ажилладаг вэ? Сүүлийн жинхэнэ incident-ийг алхам алхмаар тайлбарлаач" → Болор Jira ticket, Slack war room, post-mortem харуулна.
- **Technical verification:** "S3 bucket public болохоос яаж сэргийлдэг вэ?" → Тэмүүлэн SCP, AWS Config rule үзүүлнэ.
- **Effectiveness:** Зөвхөн "хийсэн" биш "үр дүнтэй" эсэхийг. Жишээ: "Phishing click rate буурсан уу?"

**Stage 2-д асууж болох жишээ асуултууд:**
- (DevOps-д) "Production-д код deploy хийхэд хэдэн хүн зөвшөөрөл өгөх ёстой вэ? Сүүлийн deployment-ийг үзүүлээч."
- (SOC-д) "Шөнө critical alert ирвэл хэн хариу үйлдэл хийдэг вэ? Escalation path юу вэ?"
- (IT-д) "Өчигдөр ажлаас гарсан хүний бүх хандалт хаагдсан уу? Нотлоорой."
- (Энгийн ажилтнаас) "Та ажлын мэдээллийг хувийн USB-д хуулж болох уу? Бодлого юу гэж заасан бэ?"
- (CISO-д) "Хамгийн өндөр эрсдэл юу вэ, та яаж эмчилж байна вэ?"
- (Backup) "Сүүлийн restore test хэзээ хийсэн бэ? Амжилттай байсан уу?"

**Үр дүн:** Аудитор major/minor NC, OFI бүхий тайлан гаргана.
- **Major NC** байвал сертификат олгогдохгүй — засаад дахин шалгуулна.
- **Minor NC** байвал ихэвчлэн corrective action plan гаргаж, тодорхой хугацаанд засна гэдгээ амлавал сертификат олгоно.
- **NC байхгүй / зөвхөн OFI** бол шууд зөвшөөрнө.

CloudNova-д 2 minor NC, 5 OFI гарсан. Minor NC-д 30 хоногийн дотор corrective action plan гаргаж, **ISO/IEC 27001:2022 сертификат**-аа авсан (3 жилийн хүчинтэй).

> **Бодит артефакт:** Stage 1 report, Stage 2 report, NC log, Corrective Action plans, эцэст нь **Certificate**. Аудитын турш Сараа "evidence binder" (бүх нотолгоог хялбар олдох байдлаар цэгцэлсэн) бэлдсэн — энэ нь аудитыг хурдан, гөлгөр болгодог.

---

## 14. Certification авсны дараах амьдрал

Сертификат бол төгсгөл биш, эхлэл. ISO 27001 нь "**тасралтгүй сайжруулалт**" (continual improvement)-ийн систем. Сертификат авсны дараа CloudNova дараах мөчлөгт орсон.

### Continuous Improvement (Clause 10.1)

ISMS нь амьд систем — шинэ эрсдэл, шинэ технологи, шинэ заналхийлэл гарах бүрд шинэчлэгдэнэ. Сараа улирал бүр:
- Шинэ asset бүртгэх (шинэ SaaS, шинэ микросервис).
- Шинэ эрсдэл нэмэх, хуучин эрсдэлийг дахин үнэлэх.
- Метрик хянаж, чиг хандлага муудвал арга хэмжээ авах.

### CAPA — Corrective and Preventive Action

- **Corrective action:** Зөрчил гарсны дараа үндсэн шалтгааныг (root cause) олж, засаж, дахин гарахаас сэргийлэх. Жишээ: "Access review саатсан" → шалтгаан нь "хэн хариуцахыг тодорхойлоогүй" → засвар нь "автомат сануулга + тодорхой эзэн".
- **Preventive thinking:** Зөрчил гарахаас өмнө эрсдэлийг урьдчилан илрүүлэх.
- Бүх CAPA-г **CAPA log**-д бүртгэж, root cause analysis (5 Whys, Fishbone) хийнэ.

### Internal Audit cycle

- Жил бүр дотоод аудит үргэлжилнэ — нэг удаагийн ажил биш.
- Аудитын хөтөлбөр нь бүх control-ийг 3 жилийн мөчлөгт хамруулна (эрсдэл өндөрийг илүү давтамжтай).

### Surveillance Audit

- Сертификатын байгууллага жил бүр (3 жилийн мөчлөгт 2 удаа) **surveillance audit** хийж, ISMS амьд хэвээр байгаа эсэхийг шалгана. Энэ нь Stage 2-оос бага хэмжээтэй боловч санамсаргүй хэсгүүдийг гүн шалгана.
- 3 жилийн дараа **recertification audit** (бүрэн дахин аудит).

> **Бодит артефакт:** CAPA log, шинэчлэгдсэн Risk Register, жилийн Internal Audit report, Surveillance Audit report, шинэчлэгдсэн SoA. Хамгийн том сургамж: **нотолгоо тогтмол үүсдэг системтэй компани surveillance-д амар дамждаг**; "аудитын өмнө шуурхайлдаг" компани жил бүр зовдог.

---

## Аюулгүй байдлын баг өдөр бүр яг юу хийдэг вэ? (Day-in-the-life)

ISO 27001 бол баримт биш, **өдөр тутмын зуршил**. Сертификат авсны дараа ердийн нэг өдөр CloudNova-д ингэж өрнөнө:

**08:30 — SOC аналитикч Болор:** Splunk dashboard, GuardDuty findings, шөнийн alert-ыг хянана. Сэжигтэй "impossible travel" alert олж, тухайн хэрэглэгчтэй Slack-аар баталгаажуулна (аяллын улмаас гэдгийг тогтоож, хаана). Бүх үйлдлийг тэмдэглэнэ — энэ нь monitoring control-ийн нотолгоо.

**09:00 — DevOps Тэмүүлэн:** Шинэ микросервис deploy хийх PR хянана. CI-д Trivy критик vuln олсон тул merge-ийг блоклож, dependency шинэчлэхийг хүснэ. Vault дахь нэг expire болох ойртсон секретийг rotate хийнэ.

**09:30 — IT Батбаяр:** Шинэ ажилтны onboarding — JML procedure-ийн дагуу least-privilege эрх, MFA бүртгэл, эндпойнт enrollment. Өчигдөр гарсан ажилтны бүх хандалтыг хаасан эсэхээ давхар шалгана.

**10:00 — ISMS Manager Сараа:** Энэ долоо хоногийн ISMS standup. Нээлттэй эрсдэл, treatment-ийн явц, ирэх сарын internal audit-ийн бэлтгэлийг хэлэлцэнэ. Шинэ vendor (analytics SaaS)-ийн security assessment эхлүүлэх tikет нээнэ.

**11:00 — Болор:** Энгийн ажилтан "сэжигтэй имэйл ирлээ" гэж мэдээлсэн — phishing report. Болор шинжилж, бодит phishing гэдгийг тогтоож, бусад хэн ийм имэйл авсныг Splunk-аас хайж, блоклоно. P3 incident ticket нээж, баримтжуулна.

**14:00 — Тэмүүлэн + Болор:** Улирлын **DR failover дадлага**. Secondary region руу шилжих процессыг хэмжиж (RTO/RPO баталгаажуулах), сурсан зүйлийг тэмдэглэнэ.

**16:00 — Сараа:** Vulnerability SLA dashboard шалгана. SLA-аас хэтэрсэн нэг high vuln олж, эзэнтэй нь холбогдож шалтгааныг асуун, escalate хийнэ.

**Үргэлжилсэн (автомат):** Дэвсгэрт CloudTrail лог цуглаж, Config rule-ууд compliance шалгаж, EDR хортой үйлдэл хайж, Dependabot шинэ vuln мэдээлж, backup ажиллаж байна. **Хамгийн сайн ISMS бол хүний оролцоо багатай, нотолгоо автоматаар үүсдэг систем.**

---

## 15. Бүх үйл явц — нэгдсэн Timeline

CloudNova-ийн ISO 27001 төсөл нийт **~10 сар** үргэлжилсэн. Сар бүрийн ажлыг доор харуулав. (Энэ нь ердийн хувилбар — компанийн төлөвшлөөс хамаарч 6-12 сар хэлбэлзэнэ.)

**Сар 1 — Эхлэл ба бэлтгэл**
- Management kickoff, төсөв батлах, sponsor (CISO) томилох.
- ISMS Project Team байгуулах, RACI гаргах.
- Зөвлөх/гэрчилгээжүүлэгч сонгох.
- Current state architecture, data flow зурах.
- *Артефакт:* Project Charter, kickoff minutes, RACI.

**Сар 2 — Scope ба Gap Analysis**
- ISMS Scope statement бичиж батлах.
- ISO 27001 шаардлагатай харьцуулсан **gap analysis** (бид одоо хаана байна вэ?).
- Asset inventory эхлүүлэх.
- *Артефакт:* Scope statement, gap analysis report.

**Сар 3 — Asset ба Risk Assessment**
- Asset register дуусгах (cloud, endpoint, SaaS, secrets, code).
- Risk methodology бичих.
- Risk workshop хийж, эрсдэл илрүүлж, оноожуулах.
- *Артефакт:* Asset register, Risk Register (анхны).

**Сар 4 — Risk Treatment ба SoA**
- Risk Treatment Plan гаргах (эрсдэл бүрд action, эзэн, deadline).
- Statement of Applicability (93 control) бэлдэх.
- Policy/procedure бичиж эхлэх.
- *Артефакт:* RTP, SoA v1.0, policy төслүүд.

**Сар 5-6 — Хяналт хэрэгжүүлэх (хамгийн ачаалалтай үе)**
- Техникийн hardening: MFA, PAM, SCP, WAF, branch protection, secrets, logging→SIEM.
- Policy/procedure батлах, ажилтанд танилцуулах.
- Backup, restore test, DR plan.
- Supplier assessments эхлүүлэх.
- *Артефакт:* Config screenshots, batlagдсан policies, hardening checklist.

**Сар 7 — Awareness ба тогтворжуулалт**
- Бүх ажилтны security training.
- Эхний phishing simulation.
- Incident response table-top exercise.
- Хяналтуудыг тогтмол ажиллуулж, нотолгоо хуримтлуулах.
- *Артефакт:* Training records, phishing report, table-top minutes.

**Сар 8 — Internal Audit**
- Дотоод аудит хийх (хараат бус аудитор).
- NC, OFI илрүүлж, corrective action эхлүүлэх.
- *Артефакт:* Internal audit report, NC log, CAPA.

**Сар 9 — Management Review ба Stage 1**
- Management review хурал (metrics, эрсдэл, аудитын дүн).
- Сертификатын байгууллагатай **Stage 1 audit**.
- Stage 1-ийн observation-ийг засах.
- *Артефакт:* Management review minutes, Stage 1 report.

**Сар 10 — Stage 2 ба сертификат**
- **Stage 2 certification audit**.
- Minor NC-д corrective action plan гаргах.
- **ISO/IEC 27001:2022 сертификат авах.** 🎉
- *Артефакт:* Stage 2 report, Certificate.

**Сар 11+ — Тасралтгүй мөчлөг**
- Continual improvement, CAPA.
- Дараа жилийн internal audit, surveillance audit-д бэлтгэх.
- ISMS-ийг өдөр тутмын ажлын салшгүй хэсэг болгох.

---

## Дүгнэлт: Гол сургамжууд

1. **ISO 27001 бол баримтын төсөл биш, соёлын өөрчлөлт.** Технологи нь хагас нь, хүн/процесс нь нөгөө хагас.
2. **Эрсдэлээс эхэл.** Бүх control нь эрсдэлээс үүдэлтэй байх ёстой — "ISO хэлсэн учраас" биш.
3. **Нотолгоо автоматаар үүсдэг систем байгуул.** "Аудитын өмнө шуурхайлах" нь тогтворгүй, ядаргаатай. Лог, report, ticket тогтмол үүсдэг бол аудит хялбар.
4. **Эзэмшил тодорхой байх ёстой.** Хөрөнгө, эрсдэл, control бүрт нэг хариуцагч.
5. **Удирдлагын дэмжлэг шийдвэрлэх хүчин зүйл.** Sponsor-гүй ISMS төсөл бараг үргэлж унадаг.
6. **Least privilege + MFA + logging + backup** — энэ дөрөв нь хамгийн их эрсдэлийг бууруулдаг суурь хяналтууд.
7. **Сертификат бол эхлэл.** Surveillance audit, continual improvement үргэлжилнэ.

> Энэхүү гарын авлага нь сургалтын зорилготой ерөнхий загвар бөгөөд бодит хэрэгжилт компани бүрийн нөхцөл, эрсдэл, зохицуулалтын орчноос хамаарч өөр өөр байх болно. Бодит төсөлд мэргэшсэн ISO 27001 зөвлөх болон гэрчилгээжүүлэх байгууллагатай хамтран ажиллахыг зөвлөж байна.
