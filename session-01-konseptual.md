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
## Sessiya 1 — Konseptual Əsaslar

```
Mövzular:  VCS nədir  ·  Git tarixi  ·  3-Zone Arxitektura
           .git qovluğu  ·  4 Obyekt  ·  SHA Hash
           git init  ·  git config
```

> **Data Platform & Engineering Bootcamp · DPE-01**
> Git/GitHub/GitLab Modulu · Sessiya 1/9

---

<!-- Slide 2: Agenda -->
## Bu Sessiyada Öyrənəcəksiniz

- **VCS nədir?** — 3 növ: Local, Centralized, Distributed
- **Git tarixi** — Niyə 2005-ci ildə, kim tərəfindən yaradıldı
- **3-Zone Arxitektura** — Working Directory → Staging Area → Repository
- **`.git` qovluğu** — Git-in "beyni" daxilindən baxış
- **4 Git Obyekti** — Blob, Tree, Commit, Tag
- **SHA Hash** — Data bütövlüyünün sirri
- **Local vs Remote** repo fərqi
- **`git init` + `git config`** — İlk addımlar

> Hər mövzu: konsept + analogiya + necə işləyir + ən çox edilən səhvlər + tapşırıq

---

<!-- Slide 3: VCS nədir? -->
## VCS — Versiya Nəzarəti Sistemi

**VCS** — faylların zamanla necə dəyişdiyini izləyən, "kim nə vaxt nəyi dəyişdi" sualına cavab verən sistemdir.

```
LOCAL VCS            CENTRALIZED VCS       DISTRIBUTED VCS (Git)

 [kompüter]            [MƏRKƏZ server]        [server]
     │                   ↑   ↑   ↑            ↑     ↑
   [DB]                  │   │   │            │     │
 (tarixçə)              [A] [B] [C]         [A]   [B]   [C]
                      server çöksə —       Hər klon = TAM nüsxə
                      hamı itirir!         İnternet yox? İşləyir!

  RCS, SCCS            CVS, Subversion       Git, Mercurial
```

**Git = Distributed VCS:**
- Hər klon bütün tarixçəni saxlayır
- Server olmadan tam işləyir
- Sürətli (lokal əməliyyatlar)
- Güclü branch/merge dəstəyi

---

<!-- Vizual: Git niyə lazımdır? -->
## Bunu Belə Düşün

```
  Çoxunuzun "versiya nəzarəti" deyəndə etdiyi:

    project.docx          project_final.docx
    project_v2.docx       project_FINAL_son.docx
    project_v2_yeni.docx  project_final_SON_REAL.docx

  ← bu tanış gəlir? bu dəhşətdir.

  Git ilə sadəcə BİR qovluq var:

    student-project/
    ├── main.py
    └── README.md        ← fayllar HEÇ VAXT çoxalmır

  Bütün tarixçə .git/ içindədir, görünmür, yer tutmur.

  git log göstərir:
    ● bu gün    — emaili düzəltdim
    ● dünən     — login funksiyası əlavə edildi
    ● həftə əvvəl — layihə başladı

  Üç ay əvvəlki versiyaya qayıtmaq istəyirsən?
  project_v47_REALFINAL.docx axtarmırsan artıq.
```

---

<!-- Slide 4: Git tarixi -->
## Git Tarixi — 2005

**Problem:** Linux kernel (minlərlə developer) BitKeeper VCS istifadə edirdi. 2005-ci ildə lisenziya ləğv olundu.

**Həll:** Linus Torvalds özü **2 həftəyə** Git-i yazdı:

```
2005  Git yaradıldı — Linus Torvalds (Linux kernel üçün)
      Hədəflər: sürət, distributed, güclü branch, data bütövlüyü

2007  GitHub quruldu (sosial kod platforması)

2011  GitLab quruldu (DevOps all-in-one platforma)

2018  Microsoft GitHub-u $7.5B-a satın aldı

2020  'master' → 'main' olaraq dəyişdirildi (GitHub default)

Bu gün  Dünyanın 95%+ developer-i Git istifadə edir
```

> **Qeyd:** Git — alətdir. GitHub/GitLab — bu aləti istifadə edən PLATFORMALARDIRLAR.
> Git olmadan GitHub mövcud ola bilməz. Git isə GitHub olmadan tam işləyir.

---

<!-- Slide 5: 3-Zone Arxitektura -->
## 3-Zone Arxitektura — Git-in Ürəyi

```
┌──────────────────┐  git add   ┌─────────────────┐ git commit ┌─────────────┐
│  Working         │ ─────────► │  Staging Area   │ ─────────► │ Repository  │
│  Directory       │            │  (Index)        │            │  (.git/)    │
│                  │ ◄───────── │                 │            │             │
│  Əsl fayllarınız │ git restore└─────────────────┘            │  SHA commit │
│  Dəyişiklik edir │                                            │  Daimi tarix│
└──────────────────┘                                            └─────────────┘
  "Nə dəyişdi?"           "Nəyi commit edəcəm?"           "Daimi tarixçə"
  git diff                git diff --staged                git log
```

**Köçüş analogiyası:**
- **Working Dir** = Otağınız (hər şey dağınıq, siz dəyişiklik edirsiniz)
- **Staging Area** = Qablaşdırmaq üçün hazırladığınız şeylər (seçilmişlər)
- **Repository** = Möhürlənmiş qutular (daimi, dəyişmir)

<div class="warn">⚠️ Yeni başlayanların ƏN BÖYÜK çaşqınlığı: staged vs unstaged fərqini anlamaq.</div>

---

<!-- Slide 6: .git qovluğu -->
## `.git` Qovluğunun İçi

<div class="bonus">🎁 BONUS mövzu — Git-in daxili işləmə məntiqidir, gündəlik istifadə üçün əzbərləməyə ehtiyac yoxdur</div>

```bash
$ git init
$ tree .git/

.git/
├── HEAD              # "hazırda haradayam?" — cari branch/commit
├── config            # bu repoya xüsusi konfigurasiya
├── objects/          # ← HƏR ŞEY BURADA: blob, tree, commit, tag
│   ├── a3/
│   │   └── f5e9c1b7d2...   # SHA hash (ilk 2 hex = qovluq adı)
│   ├── pack/         # sıxılmış obyektlər (böyük repo-lar üçün)
│   └── info/
├── refs/
│   ├── heads/        # local branch-lar (refs/heads/main)
│   └── tags/         # tag-lar (refs/tags/v1.0.0)
└── COMMIT_EDITMSG    # son commit mesajı

$ cat .git/HEAD
ref: refs/heads/main  # main branch-ındayam
```

<div class="warn">⚠️ `.git` qovluğunu əl ilə dəyişdirməyin və SİLMƏYİN — bütün tarixçə itirir!</div>

---

<!-- Slide 7: 4 Git Obyekti -->
## 4 Git Obyekti — Hər Şey Bu 4 Növdür

<div class="bonus">🎁 BONUS mövzu — Git-in daxili modelidir, maraqlananlar üçün</div>

```
┌─────────────┐  ┌──────────────────┐  ┌─────────────────┐  ┌──────────────┐
│    BLOB      │  │      TREE        │  │     COMMIT       │  │     TAG      │
│─────────────│  │──────────────────│  │─────────────────│  │──────────────│
│ Fayl məzmunu│  │ Qovluq strukturu │  │ Snapshot + meta │  │ Daimi etiket │
│ (fayl adı   │  │                  │  │                 │  │              │
│  yoxdur!)   │  │ main.py → blob1  │  │ author: Ali     │  │ v1.0.0       │
│             │  │ README  → blob2  │  │ date: 2025-01   │  │ → commit abc │
│ "print(x)"  │  │ src/    → tree2  │  │ tree → tree1    │  │ + mesaj      │
│ "Salam"     │  │                  │  │ parent → prev   │  │ + müəllif    │
└─────────────┘  └──────────────────┘  └─────────────────┘  └──────────────┘
```

- **Blob** — faylın MEZMUNu (adını saxlamır, yalnız məzmun)
- **Tree** — qovluq strukturu: "bu adda fayl bu blob-a işarə edir"
- **Commit** — bir zaman nöqtəsinin tam şəkli + müəllif + tarix + valideyn
- **Tag** — konkret commit-ə DAİMİ, dəyişməyən işarə

> **Eyni məzmunlu 2 fayl = 1 blob** — Git avtomatik deduplication edir!

---

<!-- Slide 8: SHA Hash -->
## SHA Hash — Git-in Barmaq İzi Sistemi

<div class="bonus">🎁 BONUS mövzu — praktik olaraq bilməli olduğunuz tək şey: hər commit-in `git log --oneline`-da görünən qısa ID-si (hash) var</div>

```bash
# Hər Git obyekti özünəməxsus SHA-1 hash-a malikdir:
$ git hash-object main.py
a3f5e9c1b7d2e8f4a1c3e5b7d9f2a4c6e8b0d2f4
# ← 40 hex simvol = 160 bit

# .git/objects/ içində bu şəkildə saxlanılır:
.git/objects/
└── a3/
    └── f5e9c1b7d2e8f4...    # ilk 2 simvol = qovluq

# Git log-da görünür:
$ git log --oneline
a3f5e9c feat: ilk commit      # yalnız 7 simvol göstərir (kifayət edir)
```

**Niyə SHA hash?**
- **Bütövlük** — bir bit dəyişsə, hash tamamilə fərqlənir
- **Unikallıq** — eyni məzmun = həmişə eyni hash
- **Sürət** — axtarış çox sürətlidir (`O(1)`)
- **Güvən** — `git fsck` ilə bütün repository yoxlanıla bilər

<div class="r">SHA hash = Git-in "verifikasiya sistemi". Dəyişdirilmiş məzmun gizlənə bilməz.</div>

---

<!-- Slide 9: git init + git config -->
## `git init` və `git config`

```bash
# ── YENİ REPO YARATMAQ ──────────────────────────────────────────
mkdir layihe-adim && cd layihe-adim
git init
# Nəticə: Initialized empty Git repository in .../layihe-adim/.git/

# Mövcud qovluqda:
git init .

# ── İDENTİFİKASİYA QURULUMU (bir dəfə, bütün sistem üçün) ──────
git config --global user.name "Adınız Soyadınız"
git config --global user.email "email@example.com"

# ── YOXLAMAQ ────────────────────────────────────────────────────
git config --list              # bütün ayarlar
git config user.name           # konkret dəyər
cat ~/.gitconfig               # global konfigurasiya faylı

# ── LOKAL OVERRIDE (yalnız bu repo üçün) ────────────────────────
git config user.email "is-email@sirket.com"
# Lokal .git/config qlobal ~/.gitconfig-u override edir
```

<div class="warn">⚠️ user.name/email konfigurasiya edilmədən commit etmək — xəta verir!</div>

---

<!-- Slide 10: Praktik Tapşırıq -->
## Praktik: İlk Git Repository

<div class="t">Tapşırıq: Yeni repo yarat, konfiqurasiya et, .git strukturunu araşdır</div>

```bash
# 1. Qovluq yarat, git-i başlat
mkdir student-project && cd student-project
git init

# 2. .git-in içinə bax
ls -la                    # .git görünür
ls .git/                  # HEAD, config, objects/, refs/
cat .git/HEAD             # ref: refs/heads/main

# 3. Konfiqurasiyanı yoxla/qur
git config --list         # user.name/email varmı?
git config --global user.name "Adın Soyadın"
git config --global user.email "email@example.com"

# 4. İlk fayl yarat (Git hələ bilmir)
echo "# Student Project" > README.md
git status                # → Untracked files: README.md
```

<div class="r">Nəticə: .git/ mövcuddur, konfiqurasiyanız var, README.md "untracked"dir — növbəti sessiyada add + commit öyrənəcəksiniz.</div>

<div class="tb">🎁 BONUS (könüllü) — `git hash-object README.md` işlədib faylın SHA hash-ını manual hesablayın.</div>

---

<!-- Slide 11: Ən Çox Edilən Səhvlər -->
## Ən Çox Edilən Səhvlər — Sessiya 1

<div class="warn">⚠️ Mövcud Git repo-nun İÇİNDƏ `git init` etmək — "repo içində repo" yaranır, `.git` içində `.git`. Çaşqınlığa səbəb olur.</div>

<div class="warn">⚠️ `user.name` / `user.email` qurmamaq — ya xəta verir, ya da yanlış müəlliflə commit olunur (şirkət repo-sunda ciddi problem).</div>

<div class="warn">⚠️ `.git` qovluğunu silmək — bütün tarixçə itirir! Working Directory (əsl fayllar) qalır, amma heç bir tarixi məlumat yoxdur.</div>

<div class="warn">⚠️ Git-i GitHub ilə eyniləşdirmək — Git = alət (lokal), GitHub = platforma (bulud). Git GitHub olmadan tam işləyir.</div>

> "Git hər şeyi `.git/` qovluğunda saxlayır. Bu qovluq olmadan — Git yoxdur. Amma əsl fayllarınız (`Working Directory`) həmişə sizin kompüterinizdədir — git init etməsəniz də, sadəcə tarixçə olmur."

---

<!-- Slide 12: Quiz -->
## Quiz — Sessiya 1

<div class="q">1. Distributed VCS-in centralized-dan əsas üstünlüyü nədir?</div>
<div class="a">Hər klon TAM tarixçəyə malikdir — server olmasa da işləyir, single point of failure yoxdur.</div>

<div class="q">2. 3-Zone Arxitekturanı izah edin.</div>
<div class="a">Working Dir: real fayllar (dəyişirsiniz). Staging: növbəti commit üçün seçilmişlər. Repository: daimi tarixçə (.git/).</div>

<div class="q">3. (BONUS) Blob obyekti nəyi saxlayır — fayl adını, yoxsa məzmunu?</div>
<div class="a">Yalnız MƏZMUNU. Fayl adı Tree obyektində saxlanılır. Eyni məzmun = eyni blob (dedup).</div>

<div class="q">4. (BONUS) SHA hash niyə data bütövlüyünü təmin edir?</div>
<div class="a">Bir bit dəyişsə hash tamamilə dəyişir — dəyişiklik gizlənə bilməz, Git avtomatik aşkarlayır.</div>

<div class="q">5. `git config --global` vs `--local` fərqi nədir?</div>
<div class="a">--global: ~/.gitconfig (sistem səviyyəsi, bütün repolar). --local: .git/config (yalnız bu repo, global-ı override edir).</div>

> **Data Platform & Engineering Bootcamp · DPE-01** · Git/GitHub/GitLab Modulu
