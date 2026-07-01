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
  section::after { color: #848688; font-size: 0.72em; }
---

<!-- Slide 1: Title -->
# Git, GitHub & GitLab Bootcamp
## Sessiya 4 — Branch, HEAD, Stash, Tag

```
Mövzular:  Branch nədir  ·  HEAD  ·  Detached HEAD
           git switch vs checkout  ·  git stash  ·  git tag
```

> **Data Platform & Engineering Bootcamp · DPE-01**
> Git/GitHub/GitLab Modulu · Sessiya 4/9

---

<!-- Slide 2: Branch nədir? -->
## Branch — Yüngül Pointer

**Branch texniki olaraq nədir?** — Sadəcə bir commit-ə işarə edən **yüngül pointer** (refs/heads/ içindəki fayl).

```
main:     C1 ──── C2 ──── C3          ← main branch C3-ə işarə edir
                    \
feature:             C4 ──── C5       ← feature branch C5-ə işarə edir

# Texniki olaraq:
cat .git/refs/heads/main
a3f5e9c1b7...    # yalnız bir SHA hash!

cat .git/refs/heads/feature-login
7b2c4d1e3f...    # başqa SHA hash
```

**Branch niyə "yüngül"dür?**
- Bir fayl + bir SHA hash = branch
- Yeni branch yaratmaq = 1 fayl yazmaq (millisaniyədə)
- SVN kimi bütün faylları kopyalamır

> **Analogy:** Branch = kitabda yer qoyduğunuz əlfəcin. Kitab (commit-lər) bir yerdədir, siz fərqli yerə baxırsınız.

---

<!-- Slide 3: git branch əmrləri -->
## `git branch` — Branch İdarəsi

```bash
# ── Siyahı ──────────────────────────────────────────────────────
git branch                     # local branch-lar (* = cari)
git branch -a                  # local + remote branch-lar
git branch -v                  # branch + son commit

# ── Yarat ───────────────────────────────────────────────────────
git branch feature-login       # branch yarat (keçmir!)
git switch -c feature-login    # yarat + dərhal keç (tövsiyə edilən)
git checkout -b feature-login  # köhnə üsul, eyni nəticə

# ── Keçid ───────────────────────────────────────────────────────
git switch main                # main-ə keç
git switch feature-login       # feature-login-ə keç

# ── Sil ─────────────────────────────────────────────────────────
git branch -d feature-login    # merge olunubsa sil
git branch -D feature-login    # məcburi sil (merge olmasa belə)

# ── Adını dəyiş ─────────────────────────────────────────────────
git branch -m kohne-ad yeni-ad  # rename
```

<div class="warn">⚠️ Branch adlandırma standartı: `feature/login`, `bugfix/header-overlap`, `hotfix/payment-crash` — aydın, proqnozlaşdırıla bilən adlar.</div>

---

<!-- Vizual: Branch = işarəci -->
## Paralel Kitab kimi Düşün

```
  Müəllif romanın son fəslini iki cür yazıb sınaqdan keçirir:

  main:              ●──●──●──●
                              \
  feature/son-fəsil:           ●──●──●
                               (alternativ bitişi sınayır)

  Hər "●" bir commit.
  Branch = sadəcə "bu commit-i göstərən bir etiket"dir.
  Faylların kopyası çıxarılmır → yaddaş xərci demək olar ki sıfır.

  git switch -c feature/son-fəsil    ← yeni etiket yarat, üstünə keç
  git switch main                    ← köhnə etikete qayıt
  → working directory DƏRHAL dəyişir (fayllar dəyişir!)

  Bəyənmədin?
  git branch -d feature/son-fəsil   ← etiket silinir, heç bir iz qalmır

  Bəyəndin?
  git merge feature/son-fəsil       ← iki yol birləşir, bir əsər olur

  HEAD = "hazırda haradasınız" göstərən ox
  git log --oneline --graph --all   ← bütün bu şəkli terminala çəkir
```

---

<!-- Slide 4: HEAD nədir? -->
## HEAD — "İndi haradayam?"

```bash
# HEAD = "cari vəziyyətin işarəsi"
cat .git/HEAD
ref: refs/heads/main          # ← normal: branch-a işarə edir

# HEAD → main → C3 (son commit)

# Branch keçdikdə HEAD dəyişir:
git switch feature-login
cat .git/HEAD
ref: refs/heads/feature-login # ← indi feature-login-ə işarə edir
```

**HEAD necə işləyir?**
```
Normal vəziyyət:
HEAD → main → C3    (HEAD branch-a işarə edir, branch commit-ə)

git log -1 --oneline
a3f5e9c (HEAD -> main) feat: son commit
         ↑              ↑
       HEAD            main branch
```

> **Xülasə:** HEAD = "siz indicə haradasınız". Normal vəziyyətdə branch-a işarə edir. Branch isə son commit-ə.

---

<!-- Slide 5: Detached HEAD -->
## Detached HEAD — "Yolu Olmayan Nöqtə"

```bash
# Normal: HEAD → main → C3
# git checkout <commit-hash> etdikdə:
git checkout a3f5e9c           # birbaşa commit-ə keç!

cat .git/HEAD
a3f5e9c1b7d2e8f4...           # ← indi BIRBAŞA commit-ə işarə edir (branch yox!)

# Git xəbərdarlığı:
# "You are in 'detached HEAD' state"
# "HEAD is now at a3f5e9c feat: login"
```

```
Detached HEAD:              Normal:
HEAD → C2 (birbaşa)        HEAD → main → C3
main → C3                  main: C1──C2──C3

Yeni commit etsəniz:        Yeni commit etsəniz:
C1──C2                     C1──C2──C3──C4
    └──C4 (HEAD)                        ↑
main: C1──C2──C3            main hərəkət edir
HEAD başqa branch-a keçsə, C4 "yetim" qalır!
```

<div class="warn">⚠️ Detached HEAD-də iş görüb branch-a keçsəniz, commit-ləriniz "yetim" qalır. Həll: `git branch yeni-branch-adi` ilə qoruyun.</div>

---

<!-- Slide 6: git switch vs git checkout -->
## `git switch` vs `git checkout`

```bash
# ── git switch (YENİ, Git 2.23+) ────────────────────────────────
git switch main                # branch-a keç (aydın, təhlükəsiz)
git switch -c feature-login    # yeni branch yarat + keç
git switch -                   # əvvəlki branch-a qayıt

# ── git checkout (KÖHNƏ, çoxməqsədli) ─────────────────────────
git checkout main              # branch-a keç (switch ilə eyni)
git checkout -b feature-login  # yeni branch yarat + keç
git checkout a3f5e9c           # commit-ə keç → Detached HEAD!
git checkout -- main.py        # faylı geri qaytar (restore ilə eyni)
```

**Niyə `switch` tövsiyə olunur?**

| Əmr | Məqsəd |
|-----|--------|
| `git switch` | Yalnız branch dəyişmək üçün |
| `git restore` | Yalnız fayl bərpası üçün |
| `git checkout` | HƏR ŞEY üçün (çaşqınlıq yaradır) |

> **Müasir yanaşma:** `git switch` + `git restore` = `git checkout`-un aydın, təhlükəsiz alternativləri.

---

<!-- Slide 7: git stash -->
## `git stash` — İşi Müvəqqəti Saxlamaq

```bash
# SSENARI: İşin ortasındasınız, TƏCİLİ başqa branch-a keçməlisiniz!

# Working dir-dəki dəyişikliklər (commit edilməmiş):
echo "yarımçıq iş" >> main.py
git status                     # modified: main.py

git stash                      # ← dəyişikliyi "anbara" qoy, WD TƏMİZLƏNİR
git status                     # nothing to commit, working tree clean

git switch hotfix-branch       # rahat keçin, iş görün
git switch main                # geri qayıdın

git stash pop                  # ← anbardan geri gətirir + anbardan silir
git stash apply                # ← geri gətirir amma ANBARDA SAXLAYIR

# ── Çoxlu stash ─────────────────────────────────────────────────
git stash list                 # bütün stash-lar
# stash@{0}: WIP on main: feat: son iş
# stash@{1}: WIP on main: başqa iş

git stash apply stash@{1}      # konkret stash-ı tətbiq et
git stash drop stash@{0}       # konkret stash-ı sil
git stash clear                # hamısını sil
```

<div class="r">Analogy: Stash = masanızdakı yarımçıq işi bir qutuya yığıb, masanı TƏMİZLƏMƏK — iş bitdikdən sonra qutudan çıxarırsınız.</div>

---

<!-- Slide 8: git tag -->
## `git tag` — Daimi Etiketlər

```bash
# ── 2 növ tag var ──────────────────────────────────────────────

# Lightweight tag — sadəcə pointer
git tag v1.0.0                 # cari commit-ə

# Annotated tag — metadata ilə (TÖVSIYƏ OLUNAN)
git tag -a v1.0.0 -m "İlk stabil reliz: tələbə layihəsi əsası hazırdır"
git tag -a v1.1.0 -m "Login funksiyası əlavə edildi"

# ── Əməliyyatlar ────────────────────────────────────────────────
git tag                        # bütün tag-lar
git show v1.0.0                # tag detalları
git tag -d v1.0.0              # local tag-ı sil

git push origin v1.0.0        # konkret tag-ı remote-a göndər
git push origin --tags         # bütün tag-ları göndər
# ⚠️ Adi git push tag-ları GÖNDƏRMİR!

# ── Tag vs Branch fərqi ─────────────────────────────────────────
# Branch → yeni commit-lərlə HƏRƏKƏT EDİR
# Tag    → həmişə eyni commit-ə QALIR (dəyişmir)
```

<div class="r">Analogy: Tag = kitabın müəyyən səhifəsinə yapışdırılan "BU NÖQTƏDİR — 1-ci Nəşr" yazılı daimi əlfəcin. Kitab böyüsə belə, əlfəcin yerlini dəyişmir.</div>

---

<!-- Slide 9: Praktik Tapşırıq -->
## Praktik Tapşırıq — Sessiya 4

<div class="t">T.1 — student-project-də `git switch -c feature/greet-update` ilə yeni branch yarat. main.py-ə funksiya əlavə edib commit et.</div>

<div class="t">T.2 — `git switch main` ilə main-ə qayıt. `git log --oneline --graph --all` ilə 2 branch-ı vizual gör.</div>

<div class="t">T.3 — main.py-ə dəyişiklik et (commit etmə). `git stash` ilə saxla. `git switch feature/greet-update`-ə keç. Geri qayıt, `git stash pop` et.</div>

<div class="t">T.4 — Köhnə bir commit hash-ını tap (`git log --oneline`). `git checkout <hash>` ilə Detached HEAD-ə keç. Xəbərdarlığı oxu. `git switch main` ilə təhlükəsiz geri qayıt.</div>

<div class="t">T.5 — `git tag -a v0.1.0 -m "İlk demo versiya"` ilə tag yarat. `git show v0.1.0` ilə detallarına bax.</div>

<div class="r">Nəticə: Branch yaratmaq, HEAD anlayışı, stash iş axını, tag — hamısı bir dəfə praktik edildi.</div>

---

<!-- Slide 10: Ən Çox Edilən Səhvlər -->
## Ən Çox Edilən Səhvlər — Sessiya 4

<div class="warn">⚠️ Detached HEAD-də commit edib, başqa branch-a keçmək — commit-lər "yetim" qalır, garbage collection ilə silinə bilər. Həll: `git branch yeni-branch`.</div>

<div class="warn">⚠️ Commit edilməmiş dəyişikliklərlə branch dəyişməyə çalışmaq — Git icazə vermir (konflikt olarsa). Həll: əvvəlcə commit et, ya stash istifadə et.</div>

<div class="warn">⚠️ `git stash`-ı unutmaq — anbarda çox sayda stash yığılır, `git stash list` ilə vaxtaşırı yoxlayın.</div>

<div class="warn">⚠️ Tag-ları adi `git push` ilə göndərməyə çalışmaq — tag-lar avtomatik göndərilmir! `git push origin --tags` lazımdır.</div>

<div class="warn">⚠️ `git checkout fayl` əvəzinə `git checkout branch` — ikisi tamamilə fərqli əmrlərdir, checkout-un çoxməqsədliliyi çaşqınlıq yaradır.</div>

---

<!-- Slide 11: Quiz -->
## Quiz — Sessiya 4

<div class="q">1. Texniki olaraq, branch nədir?</div>
<div class="a">Konkret bir commit-ə işarə edən yüngül pointer (.git/refs/heads/ içindəki fayl, yalnız SHA hash saxlayır).</div>

<div class="q">2. HEAD nəyə işarə edir?</div>
<div class="a">Adətən cari branch-a işarə edir (branch isə son commit-ə). Birbaşa commit-ə işarə edirsə — Detached HEAD.</div>

<div class="q">3. Detached HEAD-də commit etdim, necə "xilas" edim?</div>
<div class="a">`git branch yeni-branch-adi` — bu commit-i yeni branch-a "bağlar", sonra main-ə keçib merge edə bilərsiniz.</div>

<div class="q">4. `git switch` vs `git checkout` — niyə switch tövsiyə olunur?</div>
<div class="a">switch yalnız branch dəyişmək üçün nəzərdə tutulmuşdur, aydın və təhlükəsizdir. checkout çoxməqsədlidir (branch + fayl + detached HEAD) — çaşqınlıq yaradır.</div>

<div class="q">5. `git stash pop` vs `git stash apply` fərqi nədir?</div>
<div class="a">pop: geri gətirir VƏ anbardan SİLİR. apply: geri gətirir, amma anbarda SAXLAYIR.</div>

> **Data Platform & Engineering Bootcamp · DPE-01** · Git/GitHub/GitLab Modulu
