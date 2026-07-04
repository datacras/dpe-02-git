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
## Sessiya 3 — Tarixçə və Geri Qaytarma

```
Mövzular:  git log  ·  git diff  ·  git show
           git restore  ·  git reset  ·  git rm  ·  git mv
```

> **Data Platform & Engineering Bootcamp · DPE-01**
> Git/GitHub/GitLab Modulu · Sessiya 3/9

---

<!-- Slide 2: git log -->
## `git log` — Tarixçəyə Baxmaq

```bash
# ── Əsas ──────────────────────────────────────────────────────
git log                        # tam məlumat (mesaj, müəllif, tarix, hash)

git log --oneline              # qısa: hash + mesaj
# a3f5e9c feat: login əlavə edildi
# 7b2c4d1 fix: null pointer xətası
# 1e8f3a2 feat: ilk layihə strukturu

git log --oneline --graph --all   # branch-ları vizual göstər
# * a3f5e9c (HEAD -> main) feat: login
# * 7b2c4d1 fix: null pointer
# * 1e8f3a2 feat: ilk struktur

# ── Filtr ──────────────────────────────────────────────────────
git log --oneline -5           # son 5 commit
git log --oneline --author="Ali"  # müəllifə görə
git log --oneline --grep="feat"   # mesaja görə
git log --oneline -- main.py   # konkret fayl tarixçəsi
git log --stat                 # hansı fayllar dəyişdi + sətir sayı
git log -p                     # tam diff hər commit üçün
```

<div class="r">Ən çox: `git log --oneline --graph --all` — branch strukturunu vizual görmək üçün.</div>

---

<!-- Slide 3: git diff -->
## `git diff` — Fərqləri Görmək

```bash
# ── Hara baxır? ────────────────────────────────────────────────

git diff                       # Working Dir ↔ Staging Area
                               # (stage edilməmiş dəyişikliklər)

git diff --staged              # Staging Area ↔ son commit
                               # (commit ediləcəklər)

git diff HEAD                  # Working Dir ↔ son commit
                               # (staged + unstaged hər şey)

# ── Commit-lər arasında ────────────────────────────────────────
git diff a3f5e9c 7b2c4d1       # 2 commit arasındakı fərq
git diff HEAD~2 HEAD           # 2 commit əvvəl ilə indi
git diff main feature-login    # 2 branch arasındakı fərq

# ── Çıxışı oxumaq ──────────────────────────────────────────────
# --- a/main.py     ← köhnə versiya (a)
# +++ b/main.py     ← yeni versiya (b)
# @@ -10,3 +10,5 @@ ← köhnə: 10-cu sətirdən 3 sətir, yeni: 5 sətir
# -    return False  ← silindi (qırmızı)
# +    return True   ← əlavə edildi (yaşıl)
```

---

<!-- Vizual: Vaxt kapsulları -->
## Zaman Maşını Kimi Düşün

```
  ──────────────────────────────────────────────────────→ vaxt

  ●         ●         ●         ●         ●        [HEAD]
  a1b2c     d3e4f     g5h6i     j7k8l     m9n0p
  ilk       readme    login     null      style
  commit    əlavə     formu     bug fix   tamamlandı

  git log --oneline         ← bu cədvəlin terminaldakı versiyası
  git show g5h6i            ← "login formu" commitinə böyüdüb bax
  git diff d3e4f g5h6i      ← iki commit arasında nə dəyişdi?

  git restore --source=d3e4f README.md
                            ← README-ni o gününkü hala qaytar,
                               qalanına toxunma

  git reset --soft  HEAD~2  ← 2 commit geri, amma dəyişikliklər qalır
  git reset --hard  HEAD~2  ← 2 commit geri, dəyişikliklər YOX OLUR

  --hard işlətməzdən əvvəl:
  git log --oneline  → hash-i kopyala
  Bir şey yanlış getsə  git reflog  ilə geri tapırsan.
```

---

<!-- Slide 4: git show -->
## `git show` — Commit Detallarına Baxmaq

```bash
# ── Konkret commit-i görmək ─────────────────────────────────────
git show                       # son commit (HEAD)
git show a3f5e9c               # konkret commit
git show HEAD~2                # 2 commit əvvəl
git show HEAD:main.py          # o commit-dəki faylın məzmunu

# ── Çıxış nümunəsi ─────────────────────────────────────────────
$ git show a3f5e9c

commit a3f5e9c1b7d2e8f4a1c3...
Author: Ali Əslanli <ali@example.com>
Date:   Mon Jun 30 14:23:11 2025

    feat(login): email ilə giriş funksiyası əlavə edildi

    - Email formatı yoxlanılır
    - Şifrə bcrypt ilə hash olunur

diff --git a/main.py b/main.py
@@ -0,0 +1,8 @@
+def login(email, password):
+    ...
```

> `git show` = `git log -p -1 <commit>` — bir commit-in tam məzmunu

---

<!-- Slide 5: git restore -->
## `git restore` — Dəyişikliyi Geri Qaytarmaq

```bash
# ── SSENARI 1: Working Directory-ni geri qaytar ────────────────
# main.py-ni dəyişdirdim, amma xəta etdim, geri qaytarmaq istəyirəm:
git restore main.py            # ← son commit-ə qaytarır (dəyişiklik İTİR!)

# ── SSENARI 2: Staging-dən çıxar (unstage) ─────────────────────
# git add etdim, amma bu faylı bu commit-ə daxil etmək istəmirəm:
git restore --staged main.py   # ← Staging-dən çıxarır, Working Dir-də qalır

# ── SSENARI 3: Konkret commit-dən bərpa ────────────────────────
git restore --source=HEAD~3 main.py   # ← 3 commit əvvəlki versiya

# ── KÖHNƏ ÜSUL (eyni nəticə) ───────────────────────────────────
git checkout -- main.py        # ← köhnə, git restore-un etdiyi eyni şey
git reset HEAD main.py         # ← köhnə unstage üsulu
```

**`restore` vs `reset` anlayışı:**
- `git restore` → **fayl** səviyyəsindədir (Working Dir / Staging)
- `git reset` → **branch** (HEAD) səviyyəsindədir (tarixçəni geri çəkir)

<div class="warn">⚠️ `git restore main.py` — dəyişikliyi GERİ QAYTARIR, bu əməl GERİ ALINMAZ (commit olunmayıbsa)!</div>

---

<!-- Slide 6: git reset -->
## `git reset` — Tarixçəni Geri Çəkmək

<div class="bonus">🎁 BONUS: 3 rejimin fərqini bilmək faydalıdır, amma başlanğıcda sadəcə `--hard` (tam sil) və `git revert` (təhlükəsiz geri alma) kifayətdir</div>

```bash
# ── 3 rejim ────────────────────────────────────────────────────

git reset --soft HEAD~1
# HEAD bir geriyə çəkilir
# Dəyişikliklər STAGED QALIR (commit etməyə hazır)
# İstifadə: commit mesajını dəyişmək üçün

git reset --mixed HEAD~1       # DEFAULT (--mixed yazmasanız da eyni)
# HEAD bir geriyə çəkilir
# Dəyişikliklər UNSTAGED olur (Working Dir-dədir)
# İstifadə: "bu commit-i bölmək istəyirəm"

git reset --hard HEAD~1
# HEAD bir geriyə çəkilir
# Dəyişikliklər TAMAMILƏ İTİR!
# İstifadə: "son commit-i tamamilə silmək istəyirəm"
```

```
         HEAD~1  →  HEAD
           C1  →  C2  (silinəcək commit)
                  ↑
--soft:   C2-dəki dəyişikliklər STAGED qalır
--mixed:  C2-dəki dəyişikliklər UNSTAGED qalır
--hard:   C2-dəki dəyişikliklər İTİR
```

<div class="warn">⚠️ `--hard` push edilmiş commit-ə tətbiq etmək — başqalarının tarixçəsini pozur!</div>

---

<!-- Slide 7: git rm + git mv -->
## `git rm` + `git mv` — Fayl Əməliyyatları

```bash
# ── git rm ──────────────────────────────────────────────────────
git rm config.json             # faylı həm diskdən, həm index-dən silir
                               # (git add + rm əvəzinə)

git rm --cached secret.json    # YALNIZ index-dən çıxarır (disk-dəki fayl QALIR)
                               # Artıq commit olunmuş faylı .gitignore-a əlavə etdikdə!

git rm -r build/               # qovluğu rekursiv sil

# ── git mv ──────────────────────────────────────────────────────
git mv utils.py helpers.py     # fayl adını dəyiş (rename)
                               # = mv utils.py helpers.py + git rm + git add

git mv src/auth.py auth/login.py  # fayl köçür

# ── Niyə `mv` əvəzinə `git mv`? ────────────────────────────────
mv old.py new.py               # Git bunu "sil + yarat yeni" görür
git mv old.py new.py           # Git bunu "rename" kimi tanıyır (tarixçə saxlanır)

$ git status
renamed:    old.py -> new.py   # ← git mv ilə düzgün görünür
```

---

<!-- Slide 8: Praktik Tapşırıq -->
## Praktik Tapşırıq — Sessiya 3

<div class="t">T.1 — student-project-də 5 commit edib `git log --oneline --graph --all` ilə tarixçəni gör.</div>

<div class="t">T.2 — main.py-ə dəyişiklik et. `git diff` ilə unstaged-i gör. `git add` et. `git diff --staged` ilə staged-i gör.</div>

<div class="t">T.3 — main.py-ə xəta sal, `git restore main.py` ilə geri qaytar. Fərqi gör.</div>

<div class="t">T.4 — `git commit --amend` ilə son commit mesajını düzəlt. `git log` ilə hash-ın dəyişdiyini yoxla.</div>

```bash
# Yoxlama:
git log --oneline          # commit tarixçənizi görün
git show HEAD              # son commit-in detalları
```

<div class="r">Məqsəd: log/diff/restore-un hər birini bir dəfə müstəqil istifadə etmək.</div>

<div class="tb">🎁 BONUS T.5 (könüllü) — `git reset --soft HEAD~1` ilə son commit-i geri çək. `git status` ilə dəyişikliklərin staged qaldığını gör. Sonra yenidən commit et.</div>

---

<!-- Slide 9: Geri Qaytarma — Xülasə Cədvəli -->
## Geri Qaytarma — Hansı Əmr Nə Vaxt?

```
SSENARI                              HƏLL

Faylı dəyişdirdim, geri qaytarmaq   git restore <fayl>
istəyirəm (commit yoxdur)           ⚠️ DƏYİŞİKLİK İTİR

git add etdim, unstage etmək        git restore --staged <fayl>
istəyirəm                           (working dir-dəki fayl qalır)

Commit etdim, commit-i saxla        git reset --soft HEAD~1
amma staged kimi görmək istəyirəm   (dəyişikliklər staged qalır)

Commit etdim, unstage kimi görmək   git reset --mixed HEAD~1
istəyirəm (default)                 (dəyişikliklər unstaged qalır)

Commit etdim, hər şeyi silmək       git reset --hard HEAD~1
istəyirəm                           ⚠️ DƏYİŞİKLİK İTİR

Artıq push edilmiş commit-i         git revert <commit>
geri almaq (təhlükəsiz üsul)        (yeni "geri alma" commit-i yaranır)
```

<div class="warn">⚠️ `git revert` — push edilmiş commit-i geri almağın TEHLÜKƏSİZ üsuludur. `reset --hard` push edilmişlərə tətbiq etməyin.</div>

---

<!-- Slide 10: Ən Çox Edilən Səhvlər -->
## Ən Çox Edilən Səhvlər — Sessiya 3

<div class="warn">⚠️ `git diff` boş göstərir — faylı artıq stage etmisiniz! `git diff --staged` ilə baxın.</div>

<div class="warn">⚠️ `git restore main.py` əvəzinə `git checkout -- main.py` — ikisi eyni işi görür, amma `restore` (Git 2.23+) daha aydındır.</div>

<div class="warn">⚠️ `git reset --hard HEAD` — bütün staged+unstaged dəyişiklikləri silir. DİQQƏTLİ OLUN.</div>

<div class="warn">⚠️ Push edilmiş commit-i `reset --hard` ilə silmək — başqaları ilə tarixçə uyğunsuzluğu. Bunun əvəzinə `git revert` istifadə edin.</div>

<div class="warn">⚠️ `git log` çıxışından çıxa bilmirəm — `q` düyməsi ilə çıxın (less pager).</div>

---

<!-- Slide 11: Quiz -->
## Quiz — Sessiya 3

<div class="q">1. `git diff` vs `git diff --staged` fərqi nədir?</div>
<div class="a">diff: Working Dir ↔ Staging (stage edilməmişlər). diff --staged: Staging ↔ son commit (commit olunacaqlar).</div>

<div class="q">2. `git restore` vs `git reset` fərqi nədir?</div>
<div class="a">restore: fayl səviyyəsi (working dir / staging). reset: branch (HEAD) səviyyəsi, tarixçəni geri çəkir.</div>

<div class="q">3. (BONUS) `--soft`, `--mixed`, `--hard` fərqlərini izah edin.</div>
<div class="a">--soft: HEAD dəyişir, dəyişikliklər staged. --mixed: HEAD dəyişir, dəyişikliklər unstaged. --hard: HEAD dəyişir, dəyişikliklər İTİR.</div>

<div class="q">4. `git rm --cached fayl` nə edir?</div>
<div class="a">Faylı Git index-indən (izləmədən) çıxarır, amma fiziki diskdəki fayl qalır. Artıq commit olunmuş faylı .gitignore ilə gizlətmək üçün lazımdır.</div>

<div class="q">5. Push edilmiş yanlış commit-i necə geri almaq olar?</div>
<div class="a">`git revert <commit>` — yeni "geri alma" commit-i yaranır, tarixçə pozulmur. `reset --hard` push edilmişlərə TƏTBİQ ETMƏYİN.</div>

> **Data Platform & Engineering Bootcamp · DPE-01** · Git/GitHub/GitLab Modulu
