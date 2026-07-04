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
  .bonus { background: #F3E8FF; border-left: 4px solid #9333EA; padding: 4px 10px; border-radius: 0 6px 6px 0; margin: 3px 0; font-size: 0.76em; color: #581C87; font-weight: 600; }
  .tb { background: #F3E8FF; border-left: 4px solid #9333EA; padding: 4px 10px; border-radius: 0 6px 6px 0; margin: 3px 0; font-size: 0.79em; font-weight: 600; color: #581C87; }
  section::after { color: #848688; font-size: 0.72em; }
---

<!-- Slide 1: Title -->
# Git, GitHub & GitLab Bootcamp
## Sessiya 8 — Workflow Strategiyaları + Best Practices

```
Mövzular:  Git Flow  ·  GitHub Flow  ·  Trunk-Based Development
           Conventional Commits  ·  Git Best Practices
```

> **Data Platform & Engineering Bootcamp · DPE-01**
> Git/GitHub/GitLab Modulu · Sessiya 8/9

---

<!-- Slide 2: Workflow nədir? -->
## Workflow Strategiyası — Niyə Lazımdır?

**Git texniki imkanlar** verir: branch, merge, rebase...  
**Workflow strategiyası** isə — bu imkanları NECƏ İSTİFADƏ etmək lazımdır QAYDASIDIR.

```
PROBLEM:
  3 developer eyni layihədə işləyir.
  Hər biri istədiyi kimi branch açır, merge edir.
  → Xaos, ziddiyyətlər, pozulmuş main branch.

HƏLL (Workflow Strategiyası):
  Hamı EYNİ qaydaları izləyir:
  - Hansı branch-lar var?
  - Nə vaxt merge olunur?
  - Release necə hazırlanır?
  - Hotfix necə edilir?
```

**3 əsas model:**

| Model | Mürəkkəblik | Reliz tezliyi | Uyğun layihə növü |
|-------|------------|---------------|-------------------|
| Git Flow | Yüksək | Nadir, planlı | Böyük, versiyalı |
| GitHub Flow | Aşağı | Tez-tez | Veb/SaaS |
| Trunk-Based | Çox aşağı | Çox tez-tez | CI/CD mühiti |

---

<!-- Slide 3: Git Flow -->
## Git Flow — Klassik Strukturlu Model

<div class="bonus">🎁 BONUS mövzu — böyük komandalar üçündür, adını tanımaq kifayətdir</div>

**Vincent Driessen (2010)** tərəfindən təklif edilmiş, **5 branch növü** var:

```
main:        ●──────────────────●(v1.0)──────────────●(v1.0.1)
              \                  ↑                     ↑
               \           release merge          hotfix merge
                \
develop:         ●──●──●──●──●──●──●──●──●──●──●──●
                     \  /    \         /
feature/login:        ●●      \       /
                               \     /
feature/payment:                ●──●
```

**5 branch növü:**
- `main` — HƏMİŞƏ production-a hazır, stabil kod
- `develop` — növbəti reliz üçün inteqrasiya olunmuş kod
- `feature/*` — hər yeni funksiya üçün (develop-dan, develop-a)
- `release/*` — reliz hazırlığı (develop-dan, HƏM main HƏM develop-a)
- `hotfix/*` — production-da TƏCİLİ bug (main-dən, HƏM main HƏM develop-a)

<div class="warn">⚠️ Git Flow kiçik, sürətli relizli komandalar üçün ÇOXMÜRƏKKƏB ola bilər. Ağır layihələr üçündür.</div>

---

<!-- Vizual: Hansı workflow haraya? -->
## Şirkətin Ölçüsünü Düşün

```
  KİÇİK STARTUP  (2-5 dev, tez-tez deploy):

    main ← feature-a         ←── GitHub Flow
    main ← feature-b
    "PR-la main-ə qayıdar, hər gün deploy"

  ─────────────────────────────────────────────────────

  BÖYÜK KORPORASIYA  (50+ dev, planlı relizlər):

    main ← release/2.1 ← develop ← feature-*  ←── Git Flow
    "hər şeyin branch-ı, planı, icazəsi var"
    Ağırdır, amma nəzarət maksimaldır.

  ─────────────────────────────────────────────────────

  DEVOPs ŞİRKƏTİ  (güclü CI/CD, avtomatik testlər):

    main ← hər gün onlarca kiçik commit  ←── Trunk-Based
    "test yoxdursa başlama"

  ─────────────────────────────────────────────────────

  Heç biri "ən yaxşı" deyil.
  Kiçik komanda Git Flow işlatsа → iş yavaşlar, frustrasiya.
  Böyük komanda GitHub Flow işlatsа → xaos, kim nə etdi bilmir.

  İş tapanda soruş: "burada hansı branching strategiyası var?"
```

---

<!-- Slide 4: Git Flow Praktik -->
## Git Flow — Praktik Nümunə

<div class="bonus">🎁 BONUS mövzu</div>

```bash
# ── Qurulum ────────────────────────────────────────────────────
git switch -c develop main    # develop branch-ı yarat

# ── Feature işi ────────────────────────────────────────────────
git switch -c feature/login develop    # develop-dan ayrıl
# ... işlə ...
git switch develop
git merge feature/login        # develop-a qayıt
git branch -d feature/login

# ── Reliz hazırlığı ────────────────────────────────────────────
git switch -c release/1.0 develop      # develop-dan ayrıl
# ... son testlər, kiçik düzəlişlər ...
git switch main
git merge release/1.0
git tag -a v1.0.0 -m "İlk stabil reliz"
git switch develop
git merge release/1.0                  # develop-a da merge!
git branch -d release/1.0

# ── Hotfix ─────────────────────────────────────────────────────
git switch -c hotfix/null-crash main   # main-dən ayrıl
git commit -am "fix: null pointer həll edildi"
git switch main && git merge hotfix/null-crash
git tag -a v1.0.1 -m "Kritik bug həll edildi"
git switch develop && git merge hotfix/null-crash
```

---

<!-- Slide 5: GitHub Flow -->
## GitHub Flow — Sadə, Müasir Model

**Yalnız 2 növ branch:** `main` + qısa ömürlü `feature/*`

```
main:       ●──●──●──●──●──●──●    ← HƏMİŞƏ deploy oluna bilən
             \   /\   /\    /
feature/A:    ●─●  \   /\  /
                    \ /  \/
feature/B:           ●   ●
              (hər feature → PR → review → merge → DƏRHAL deploy)
```

```bash
# ── GitHub Flow ardıcıllığı ────────────────────────────────────
git switch main
git pull origin main                   # ən son vəziyyəti al

git switch -c feature/add-search
# ... iş gör, commit et ...
git push -u origin feature/add-search

# GitHub-da: Pull Request aç → review → merge → deploy
# CI/CD avtomatik işləyir

git switch main && git pull            # merge-dən sonra yenilə
git branch -d feature/add-search      # köhnə branch-ı sil
```

> **Fəlsəfə:** "Kiçik addımlarla, TEZ-TEZ irəlilə." main HƏMİŞƏ DEPLOY OLUNABİLƏN vəziyyətdə olmalıdır.

---

<!-- Slide 6: Trunk-Based Development -->
## Trunk-Based Development — Ən Sürətli Model

<div class="bonus">🎁 BONUS mövzu — güclü CI/CD infrastrukturu tələb edir, adını tanımaq kifayətdir</div>

```
trunk/main:   ●●●●●●●●●●●●●●●●    ← hər developer BİRBAŞA commit edir
               \  \  \  \
               kiçik, çox qısa branch-lar (saatlar, ən çox 1-2 gün)
```

**Əsas prinsiplər:**
- Bütün developer-lər HƏR GÜN main-ə commit edir
- Uzunmüddətli branch-lar YOXDUR
- Yarımçıq funksiyalar **Feature Flag** ilə gizlədilir
- Güclü avtomatik test + CI pipeline ŞƏRT

```bash
# ── TBD günlük iş axını ───────────────────────────────────────
git switch main && git pull origin main
# ... kiçik, fokuslanmış dəyişiklik ...
git add . && git commit -m "feat: axtarış düyməsinə aria-label əlavə edildi"
git push origin main     # (və ya çox qısa branch + sürətli PR)
```

<div class="warn">⚠️ TBD güclü avtomatik test infrastrukturu OLMADAN RİSKLİDİR — hər commit birbaşa main-ə gedir. DevOps mühəndisi kimi rolunuz məhz bu CI/CD avtomatlaşmanı qurmaq.</div>

---

<!-- Slide 7: Conventional Commits -->
## Conventional Commits — Standart Commit Mesajları

**Format:**
```
<tip>(<əhatə>): <qısa təsvir>

[ətraflı izah — könüllü]

[BREAKING CHANGE: ... — könüllü]
```

**Əsas tiplər:**
```bash
git commit -m "feat(login): email ilə giriş funksiyası əlavə edildi"
git commit -m "fix(api): 500 xətasına səbəb olan null pointer düzəldildi"
git commit -m "docs(readme): API açarı qurulumu izahı əlavə edildi"
git commit -m "refactor(db): sorğu funksiyaları optimallaşdırıldı"
git commit -m "test(auth): giriş ssenarisi üçün test əlavə edildi"
git commit -m "chore: .gitignore-a __pycache__ əlavə edildi"
git commit -m "feat!: yeni API versiyası, köhnə endpoint-lər dəstəklənmir"
# ↑ ! = BREAKING CHANGE
```

**Niyə vacibdir?**
- Commit tarixçəsi **özü-özünü izah edir**
- `semantic-release` ilə versiya nömrəsi AVTOMATIK artır
- CHANGELOG avtomatik yaradıla bilir
- CI/CD pipeline-ları commit tipinə görə fərqli davrana bilər

---

<!-- Slide 8: Git Best Practices (10 Qayda) -->
## Git Best Practices — 10 Peşəkar Qayda

```
1.  Kiçik, TEZ-TEZ, FOKUSLANMIŞ commit-lər — 1 commit = 1 məqsəd

2.  Mənalı commit mesajları yazın (Conventional Commits standartı)

3.  main/master-a HEÇ VAXT birbaşa commit etməyin
    — həmişə branch + PR/MR prosesi

4.  Push etməzdən ƏVVƏL git pull/fetch edin
    — konfliktləri ERKƏN aşkarlayın

5.  Həssas məlumatları (API açarı, şifrə) HEÇ VAXT commit etməyin
    — .gitignore + environment variables istifadə edin

6.  Paylaşılmış (push edilmiş) tarixçəni REBASE/RESET etməyin
    — komanda yoldaşlarının tarixçəsini pozur

7.  Tag-lardan relizləri işarələmək üçün istifadə edin (v1.0.0)

8.  git status və git diff-i TEZ-TEZ yoxlayın
    — "nəyi commit edirəm" sualına HƏMİŞƏ aydın cavab verin

9.  Branch adlarını AYDIN, STANDART formatda yazın
    — feature/login, bugfix/header-overlap, hotfix/payment-crash

10. Köhnə, istifadə olunmayan branch-ları silin
    — git branch -d (merged), git push origin --delete <branch>
```

---

<!-- Slide 9: Workflow Müqayisəsi -->
## Hansı Workflow Nə Vaxt?

```
SİZİN VƏZIYYƏT                    TÖVSİYƏ

Böyük komanda, planlı relizlər     Git Flow
(mobil tətbiq, korporativ sistem)  (develop + release + hotfix)

Kiçik-orta komanda                 GitHub Flow
Tez-tez deploy                     (main + feature + PR)
(veb tətbiq, SaaS)

CI/CD mədəniyyəti var              Trunk-Based Development
Güclü test infrastrukturu          (main-ə gündəlik commit)
(böyük texnologiya şirkətləri)

AÇIQ MƏNBƏ LAYİHƏSİ               Fork + PR workflow
(başqasının repo-suna töhfə)       (fork → clone → upstream)
```

> **Əməli məsləhət:** Real iş mühitində şirkətdən şirkətə dəyişir. Hər ikisini (GitHub Flow + Git Flow) bilmək sizi istənilən komandaya hazır edir. Trunk-Based — DevOps mühitinin gələcəyidir.

---

<!-- Slide 10: Praktik Tapşırıq -->
## Praktik Tapşırıq — Sessiya 8

<div class="t">T.1 — GitHub Flow simulyasiyası: main-dən 2 feature branch yarat. Hər birini ayrıca PR ilə main-ə birləşdir. `git log --graph` ilə tarixçəni gör.</div>

<div class="t">T.2 — Conventional Commits: student-project-dəki son 5 commit-in mesajını `git log --oneline` ilə baxın. Onları Conventional Commits formatına uyğun yenidən yazın (yeni commit-lərlə). Hər tipdən: feat, fix, docs, chore.</div>

```bash
git log --oneline --graph --all   # bütün branch strukturu
git tag                           # tag-lar
```

<div class="r">T.1 (GitHub Flow) real iş axınıdır — bunu tam mənimsəyin.</div>

<div class="tb">🎁 BONUS T.3 (könüllü) — Git Flow simulyasiyası: main + develop branch-ı yarat. feature/login develop-dan ayrılsın, işlənsin, develop-a merge edilsin. release/1.0 branch-ı yaradılıb main-ə merge edilsin, tag qoyulsun, develop-a da merge edilsin.</div>

---

<!-- Slide 11: Quiz -->
## Quiz — Sessiya 8

<div class="q">1. (BONUS) Git Flow-da neçə əsas branch növü var? Adlarını sayın.</div>
<div class="a">5 növ: main, develop, feature/*, release/*, hotfix/*.</div>

<div class="q">2. GitHub Flow-da `main` branch-ın HƏMİŞƏ hansı vəziyyətdə olması tələb olunur?</div>
<div class="a">HƏMİŞƏ deploy oluna bilən (production-a hazır) vəziyyətdə. Bu modelin ƏN VACİB şərtidir.</div>

<div class="q">3. (BONUS) Trunk-Based Development niyə güclü avtomatik test tələb edir?</div>
<div class="a">Çünki hər commit birbaşa main-ə gedir. Testlər olmadan hər commit production-u poza bilər. CI pipeline = TBD-nin "qoruma yastığı".</div>

<div class="q">4. Conventional Commits-də `feat` vs `fix` fərqi nədir?</div>
<div class="a">feat — yeni funksiya (MINOR versiya artımı). fix — bug düzəlişi (PATCH versiya artımı). BREAKING CHANGE → MAJOR versiya artımı.</div>

<div class="q">5. Niyə birbaşa `main`-ə commit etmək tövsiyə olunmur?</div>
<div class="a">Kod review prosesini, keyfiyyət nəzarətini ATLAYIR. Riskli dəyişikliklərin qarşısını ala bilmir. Komanda əməkdaşlığını zəiflədir.</div>

> **Data Platform & Engineering Bootcamp · DPE-01** · Git/GitHub/GitLab Modulu
