---
marp: true
theme: default
paginate: true
backgroundColor: "#F7F9F8"
color: "#0F172A"
style: |
  section {
    font-family: 'Segoe UI', Arial, sans-serif;
    padding: 22px 42px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    box-sizing: border-box;
    overflow: hidden;
    background-color: #F7F9F8;
    color: #0F172A;
  }
  h1 { color: #00A859; font-size: 1.55em; margin: 0 0 6px 0; }
  h2 { color: #00A859; font-size: 1.1em; border-bottom: 2px solid #00A859; padding-bottom: 4px; margin: 0 0 8px 0; }
  strong { color: #00A859; }
  em { color: #848688; font-style: normal; }
  code { background: #E8F5EE; color: #00703A; padding: 2px 5px; border-radius: 4px; font-size: 0.75em; border: 1px solid #C3E4D0; }
  pre { background: #0F172A; color: #E2E8F0; padding: 7px 11px; border-radius: 8px; border-left: 4px solid #00A859; font-size: 0.44em; line-height: 1.35; margin: 3px 0; overflow: hidden; }
  pre code { font-size: 1em; padding: 0; background: transparent; color: #A7F3D0; border: none; }
  blockquote { border-left: 4px solid #00A859; background: #E8F5EE; padding: 6px 12px; margin: 5px 0; border-radius: 0 8px 8px 0; color: #0F172A; font-style: normal; font-size: 0.78em; }
  ul { line-height: 1.5; margin: 3px 0; padding-left: 18px; }
  li { margin-bottom: 2px; font-size: 0.80em; }
  table { font-size: 0.72em; width: 100%; border-collapse: collapse; margin: 5px 0; }
  th { background: #00A859; color: white; padding: 5px 8px; text-align: left; }
  td { padding: 4px 8px; border-bottom: 1px solid #C3E4D0; }
  tr:nth-child(even) { background: #F0FAF4; }
  .t { background: #EFF6FF; border-left: 4px solid #3B82F6; padding: 4px 10px; border-radius: 0 6px 6px 0; margin: 3px 0; font-size: 0.79em; font-weight: 600; color: #1E3A5F; }
  .r { background: #F0FAF4; border-left: 4px solid #00A859; padding: 4px 10px; border-radius: 0 6px 6px 0; margin: 1px 0 5px 0; font-size: 0.76em; color: #0F172A; }
  .warn { background: #FFF8E1; border-left: 4px solid #F59E0B; padding: 5px 10px; margin: 5px 0; border-radius: 0 6px 6px 0; font-size: 0.76em; color: #78350F; }
  .q { background: #EFF6FF; border-left: 4px solid #3B82F6; padding: 5px 10px; margin: 4px 0; border-radius: 0 6px 6px 0; font-size: 0.78em; font-weight: 600; color: #1E3A5F; }
  .a { background: #F0FAF4; border-left: 4px solid #00A859; padding: 4px 10px; border-radius: 0 6px 6px 0; margin: 1px 0 6px 0; font-size: 0.76em; color: #0F172A; }
  section::after { color: #848688; font-size: 0.72em; }
---

<!-- Slide 1: Title -->
# Git, GitHub & GitLab Bootcamp
## Sessiya 7 — GitHub, GitLab, Pull Request / Merge Request

```
Mövzular:  Pull Request · Merge Request
           GitHub nədir  ·  GitLab nədir
           GitHub vs GitLab müqayisəsi
```

> **Data Platform & Engineering Bootcamp · DPE-01**
> Git/GitHub/GitLab Modulu · Sessiya 7/9

---

<!-- Slide 2: PR/MR nədir? -->
## Pull Request / Merge Request — Kod Review Mexanizmi

**PR (GitHub) = MR (GitLab)** — eyni konsepsiyanın iki fərqli adı:

> "Mənim bu branch-dakı işimi, zəhmət olmasa **nəzərdən keçirin** və əsas branch-a **birləşdirin**"

```
feature-login branch  ●──●──●──●
                                \
                            PR/MR açılır
                                │
                        ┌───────┴────────┐
                        │ Review         │
                        │ Şərhlər        │
                        │ CI/CD test     │
                        │ Approve        │
                        └───────┬────────┘
                                │ Merge
                                ▼
main branch           ●──●──●──●──●(merge commit)
```

**PR/MR Git-in öz əmri DEYİL** — GitHub/GitLab platformasının bir funksiyasıdır.

> **Analogy:** PR = jurnalda məqalə dərc etmək istəyən yazıçının prosesi: yazar → redaksiyaya göndər → redaktorlar oxuyur, şərh yazır → qəbul/rədd.

---

<!-- Slide 3: PR/MR Yaratmaq -->
## PR/MR Yaratmaq — Addım-addım

```bash
# ── Terminal hissəsi (hər zaman) ──────────────────────────────
git switch -c feature/login-form
# ... iş görün, commit edin ...
git push -u origin feature/login-form

# ── GitHub veb interfeysi ─────────────────────────────────────
# 1. Repo-ya gedin → "Compare & pull request" düyməsi görünür
# 2. Base branch: main ← Compare: feature/login-form
# 3. Başlıq yazın: "feat: login formu əlavə edildi"
# 4. Təsvir yazın: nə etdiniz, niyə, test necə
# 5. Reviewer(lər) seçin
# 6. "Create pull request" düyməsinə basın

# ── GitHub CLI (terminal-dan PR) ─────────────────────────────
gh pr create \
  --title "feat: login formu əlavə edildi" \
  --body "Bu PR login funksiyasını əlavə edir. Test: manual."

gh pr list             # açıq PR-lar
gh pr view <number>    # PR detalları
gh pr merge <number>   # terminal-dan merge
```

<div class="r">Yaxşı PR: kiçik, fokuslanmış (1 feature = 1 PR), aydın başlıq, testləri keçir, şərhlərə cavab verilir.</div>

---

<!-- Vizual: PR = kod qapısı -->
## Kod Qapısı kimi Düşün

```
  feature branch   →   PR/MR açırsan   →   review   →   merge
  ───────────────      ───────────────      ──────────    ──────

  SƏN:                      KOMANDA:
  ─────────────────         ─────────────────────────────────────
  kod yazdın                "bu dəyişikliyi niyə etdin?"
  push etdin                "bu edge case-i test etmisən?"
  PR açdın                  "burada daha yaxşı yol var"
                            Approve ✓

  Sonra SƏN:  düzəldirsən → push et → PR özü-özünü yeniləyir

  Nəhayət:   merge → feature branch silinir → hamı istifadə edir


  PR olmayan komanda:   kim nə yazdı bilmir, pozulmuş kod main-ə gedir
  PR olan komanda:      hər dəyişiklik ən azı 1 nəfər tərəfindən baxılır


  Şərhlər şəxsi hücum deyil — kod haqqındadır.
  Reviewer-in fikrinə "sən yanılırsan" yox,
  "başa düşdüm, belə düzəldim" ilə cavab vermək
  yaxşı mühəndis nişanəsidir.
```

---

<!-- Slide 4: PR/MR Review Prosesi -->
## PR/MR Review — Komanda Əməkdaşlığı

```bash
# ── Reviewer kimi ─────────────────────────────────────────────
# PR-ı açın → "Files changed" tabına keçin
# Hər sətri yorum yaza bilərsiniz
# 3 nəticə:
#   - Comment: fikir bildirin
#   - Approve: hazırdır, merge edə bilərsiniz
#   - Request changes: bu dəyişikliklər lazımdır

# ── Müəllif kimi (şərhlərə cavab) ────────────────────────────
git switch feature/login-form
# şərhlərə uyğun dəyişikliklər et
git commit -m "fix: reviewer şərhinə əsasən email validasiyası əlavə edildi"
git push origin feature/login-form
# PR avtomatik yenilənir

# ── Merge variantları ─────────────────────────────────────────
# Create a merge commit  → Three-Way merge commit yaranır (tarixçə saxlanır)
# Squash and merge       → bütün commit-lər 1-ə sıxılır (təmiz tarixçə)
# Rebase and merge       → commit-lər rebased edilir (düz xətt)
```

<div class="warn">⚠️ Böyük, çox şeyi əhatə edən PR/MR-lar review üçün çətin olur — kiçik, fokuslanmış PR/MR-lar tövsiyə olunur. Review şərhlərinə cavab vermədən sükutla davam etmək — pis əməkdaşlıq mədəniyyətidir.</div>

---

<!-- Slide 5: GitHub nədir? -->
## GitHub — Git Hosting Platforması

**GitHub** (2008, Microsoft 2018) — Git repository-lərini buluda saxlamaq, paylaşmaq və komanda ilə əməkdaşlıq üçün veb əsaslı platformadır.

```
┌─────────────────────────────────────────────────────────────────┐
│                         GitHub                                  │
│                                                                 │
│  Repos (Git)  │  Issues  │  Actions  │  Pages  │  Projects     │
│               │ (tapşırıq│ (CI/CD    │(statik  │ (Kanban       │
│  kod saxlama  │  bug izlə│  avtom.)  │ hosting)│  board)       │
└─────────────────────────────────────────────────────────────────┘
          ↑ standart Git protokolu (HTTPS/SSH)
          │
[Sizin lokal repo]
```

**Əsas funksiyalar:**
- **Pull Request** — kod review və birləşdirmə
- **GitHub Actions** — CI/CD avtomatlaşdırma (`.github/workflows/`)
- **GitHub Pages** — pulsuz statik sayt hosting
- **Issues** — bug/tapşırıq izləmə
- **Projects** — Kanban-tipli idarəetmə

> Git olmadan GitHub mövcud ola bilməz, amma **Git GitHub OLMADAN da tam işləyir**.

---

<!-- Slide 6: GitLab nədir? -->
## GitLab — Tam DevOps Platforması

**GitLab** (2011, GitLab Inc.) — Git hosting + built-in CI/CD + monitoring + tam DevOps həyat dövrü.

```
┌──────────────────────────────────────────────────────────────────┐
│                          GitLab                                  │
│                                                                  │
│  Repos  │  Issues  │  CI/CD      │  Container  │  Monitoring   │
│  (Git)  │  Boards  │  Pipeline   │  Registry   │               │
│         │          │(.gitlab-ci) │             │               │
│              Tam DevOps həyat dövrü (Plan → Deploy)             │
└──────────────────────────────────────────────────────────────────┘
```

**GitLab-ın fərqləndirici xüsusiyyətləri:**
- **Daxili CI/CD** — `.gitlab-ci.yml` ilə (əvvəldən inteqrasiya olunub)
- **Self-hosted** — öz serverinizdə quraşdırıla bilər (Community Edition pulsuz)
- **Merge Request** — PR-ın GitLab adı
- **Container Registry** — Docker image-ləri saxlamaq üçün
- **Auto DevOps** — avtomatik CI/CD pipeline-lar

> **DevOps Bootcamp əhəmiyyəti:** GitLab CI/CD real iş mühitindəki DevOps pipeline-ların əsasıdır.

---

<!-- Slide 7: GitHub vs GitLab Müqayisəsi -->
## GitHub vs GitLab — Xülasə Cədvəli

| Xüsusiyyət | GitHub | GitLab |
|------------|--------|--------|
| **Əsas fokus** | Açıq mənbə icması, sosial paylaşım | Tam DevOps həyat dövrü (Plan→Deploy) |
| **CI/CD** | GitHub Actions (sonradan əlavə) | GitLab CI/CD (əvvəldən daxili) |
| **Self-hosted** | GitHub Enterprise (ödənişli) | Community Edition (pulsuz) |
| **Termin** | Pull Request (PR) | Merge Request (MR) |
| **CI konfiq** | `.github/workflows/*.yml` | `.gitlab-ci.yml` |
| **Sahiblik** | Microsoft | GitLab Inc. (açıq mənbə şirkəti) |
| **İcma ölçüsü** | Daha böyük açıq mənbə icması | Daha çox korporativ/daxili istifadə |

**Nəticə:** Eyni Git protokolunu istifadə edirlər — bir lokal repo-nu HƏR İKİ platformaya push etmək mümkündür:

```bash
git remote add github https://github.com/user/repo.git
git remote add gitlab https://gitlab.com/user/repo.git
git push github main && git push gitlab main
```

---

<!-- Slide 8: GitLab CI/CD Giriş -->
## GitLab CI/CD — `.gitlab-ci.yml` Giriş

```yaml
# .gitlab-ci.yml — repo kökündə
stages:
  - test
  - build
  - deploy

test-job:
  stage: test
  image: python:3.11
  script:
    - pip install -r requirements.txt
    - python -m pytest tests/
  only:
    - merge_requests
    - main

build-job:
  stage: build
  script:
    - echo "Build prosesi..."
  only:
    - main

deploy-job:
  stage: deploy
  script:
    - echo "Deploy prosesi..."
  only:
    - main
  when: manual
```

> Hər commit/push-da `.gitlab-ci.yml` avtomatik işləyir. PR/MR-da test keçmədən merge mümkün deyil.

---

<!-- Slide 9: Praktik Tapşırıq -->
## Praktik Tapşırıq — Sessiya 7

<div class="t">T.1 — student-project-də `feature/readme-update` branch-ı yaradın. README.md-ə bölmə əlavə edib push edin. GitHub-da Pull Request açın (base: main ← compare: feature/readme-update).</div>

<div class="t">T.2 — Özünüz PR-ı "review" edin: "Files changed"-ə bakın, bir sətirə şərh yazın. Sonra "Approve" edin, "Merge pull request" ilə main-ə birləşdirin.</div>

<div class="t">T.3 — Eyni layihəni GitLab-a da push edin (ikinci remote olaraq). `git remote add gitlab <gitlab-url>` + `git push gitlab main`.</div>

<div class="t">T.4 — GitLab-da sadə `.gitlab-ci.yml` əlavə edin (yalnız `echo "test keçdi"` olan 1 stage). Push edin, CI pipeline-ın işlədiyini görün.</div>

```bash
# Yoxlama:
git remote -v              # həm github, həm gitlab görünür?
gh pr list                 # GitHub-da PR-lar
```

<div class="r">T.2 ən vacibdir — PR yaratmaq + merge etmək gündəlik DevOps iş axınıdır.</div>

---

<!-- Slide 10: Ən Çox Edilən Səhvlər -->
## Ən Çox Edilən Səhvlər — Sessiya 7

<div class="warn">⚠️ PR/MR-dan əvvəl branch-ı remote-a push etməyi unutmaq — platforma yalnız remote-dakı branch-lar arasında PR/MR yarada bilər.</div>

<div class="warn">⚠️ Çox böyük PR/MR yaratmaq — 500+ sətir dəyişikliyi reviewer üçün çox çətindir. Kiçik, fokuslanmış PR/MR-lar (1 feature = 1 PR) daha yaxşıdır.</div>

<div class="warn">⚠️ Review şərhlərinə cavab vermədən sükutla davam etmək — komanda mədəniyyəti pozulur. Hər şərhi cavablandırın.</div>

<div class="warn">⚠️ GitHub-u Git ilə qarışdırmaq — Git = alət (lokal, pulsuz, hər yerdə işləyir). GitHub = platforma (bulud, şirkət məhsulu).</div>

<div class="warn">⚠️ GitLab CI konfiq faylının adını yanlış yazmaq — mütləq `.gitlab-ci.yml` (nöqtə ilə başlayır, YAML uzantısı).</div>

---

<!-- Slide 11: Quiz -->
## Quiz — Sessiya 7

<div class="q">1. PR (GitHub) vs MR (GitLab) fərqlidir, yoxsa eyni konsepsiya?</div>
<div class="a">Eyni konsepsiyadır — "mənim branch-ımı nəzərdən keçirin və birləşdirin" mesajı. Yalnız adları fərqlidir: GitHub "Pull Request", GitLab "Merge Request" deyir.</div>

<div class="q">2. PR/MR Git-in öz əmridirmi?</div>
<div class="a">Xeyr — GitHub/GitLab kimi platformaların funksiyasıdır. Git özündə PR/MR anlayışı yoxdur.</div>

<div class="q">3. GitLab-ın GitHub-a əsas fərqləndirici xüsusiyyəti nədir?</div>
<div class="a">Tam DevOps həyat dövrünü bir platformada birləşdirir (Plan → kod → test → deploy → monitor). Daxili CI/CD (.gitlab-ci.yml) əvvəldən mövcuddur, self-hosted Community Edition pulsuz.</div>

<div class="q">4. Bir lokal repo-nu həm GitHub-a, həm GitLab-a push etmək olarmı?</div>
<div class="a">Bəli — `git remote add github <url>` + `git remote add gitlab <url>`, sonra hər birinə ayrıca push. Git eyni protokolu istifadə edir.</div>

<div class="q">5. GitHub Actions vs GitLab CI fərqi nədir?</div>
<div class="a">GitHub Actions `.github/workflows/*.yml` — sonradan əlavə edilmiş, daha geniş ekosistem. GitLab CI `.gitlab-ci.yml` — əvvəldən daxili, güclü inteqrasiya.</div>

> **Data Platform & Engineering Bootcamp · DPE-01** · Git/GitHub/GitLab Modulu
