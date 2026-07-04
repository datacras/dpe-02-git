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
## Sessiya 5 — Merge, Conflict, Rebase, Cherry-Pick

```
Mövzular:  Fast-Forward Merge  ·  Three-Way Merge
           Merge Conflict  ·  Rebase  ·  Cherry-Pick
```

> **Data Platform & Engineering Bootcamp · DPE-01**
> Git/GitHub/GitLab Modulu · Sessiya 5/9

---

<!-- Slide 2: Merge — Ümumi Konsept -->
## `git merge` — Branch-ları Birləşdirmək

```bash
# ── Əsas sintaksis ─────────────────────────────────────────────
git switch main                # HARa gətiririk? main-ə
git merge feature-login        # NƏYİ gətiririk? feature-login-i
# ← "feature-login branch-ındakı dəyişiklikləri MƏNƏ GƏTİR"

git merge --no-ff feature-login   # Merge commit-i MƏCBUR yarat
git merge --abort                 # Merge zamanı geri çək
```

**Merge 2 növdür:**

```
NƏ VAXT?                         NÖV

main dəyişməyibsə               Fast-Forward (asan, merge commit yox)
(feature main-dən ayrılandan
sonra main commit etməyib)

main VƏ feature ikisi           Three-Way Merge (merge commit yaranır)
dəyişibsə
(paralel inkişaf)
```

> **Mövzu yaddasaxlama:** `git merge feature` o deməkdir ki, hazırda `main`-də olub, `feature`-dəki dəyişiklikləri GƏTİRİRƏM. İstiqamət çox vacibdir!

---

<!-- Slide 3: Fast-Forward Merge -->
## Fast-Forward Merge — Ən Sadə Birləşmə

```
ƏVVƏL (merge-dən önce):
main:    ●──●                    ← main bu nöqtədə DAYANIBa (C2)
              \
feature:       ●──●──● (HEAD)   ← feature C2-dən sonra inkişaf edib (C3,C4,C5)

git switch main
git merge feature

SONRA (Fast-Forward):
main:    ●──●──●──●──●           ← main sadəcə "irəli sürüşdü"
                       ↑
              main VƏ feature indi EYNİ nöqtəyə işarə edir
              YENİ MERGE COMMIT YARANMADI!
```

```bash
git switch main
git switch -c feature-x
echo "yeni funksiya" >> main.py
git commit -am "feat: feature-x əlavə edildi"

git switch main       # main bu vaxt dəyişməyib
git merge feature-x   # Fast-Forward baş verir, "Fast-forward" mesajı görünür
git log --oneline     # yalnız feature-x commit-ləri, əlavə merge commit yoxdur
git branch -d feature-x
```

<div class="r">Fast-Forward = Git-in "ən ağıllı" hərəkətidir. Əgər 2 yol arasında HEÇ BİR ziddiyyət yoxdursa, sadəcə irəli gedir — əlavə iş görmür.</div>

---

<!-- Vizual: Merge növləri -->
## İki Yol Birləşir

```
  FAST-FORWARD (ən sadə):
  main dayanıb bekliyordu, feature irəli getmişdi:

  main:     ●──●
                 \
  feature:        ●──●──●

  merge:    ●──●──●──●──●   ← main sadəcə "irəli atladı"
                               yeni commit YOX, tarixçə düz xətt

  ───────────────────────────────────────────────────

  THREE-WAY (hər iki tərəf dəyişibsə):

  main:     ●──●──●──●
                  \      \
  feature:         ●──●───● ← bu yeni commit 2 valideynə malikdir

  ───────────────────────────────────────────────────

  KONFLİKT (eyni sətir hər iki tərəfdən dəyişdirilibsə):

  <<<<<<< HEAD
  return "salam"          ← main-dəki versiyadır
  =======
  return "hello"          ← feature-dəki versiyadır
  >>>>>>> feature/english

  Git nə seçəcəyini bilmir, SƏNİ soruşur.
  Qərar ver, işarələri sil, git add, git commit.
```

---

<!-- Slide 4: Three-Way Merge -->
## Three-Way Merge — Paralel İnkişaf

```
main VƏ feature hər ikisi dəyişibsə:

         ortaq əcdad (C2)
              |
    ┌─────────┴──────────┐
    │                    │
main: ●──●(C3)      feature: ●──●(C4)

git switch main
git merge feature     → Three-Way Merge!

Nəticə:
main: ●──●(C1)──●(C2)──●(C3)──────●(MERGE COMMIT: 2 valideyni var!)
                         \         /
                          ●──●(C4)
                  feature
```

```bash
git switch main
echo "main dəyişdi" >> README.md
git commit -am "docs: README main-də yeniləndi"

git switch feature-login
echo "feature dəyişdi" >> main.py
git commit -am "feat: login əlavə edildi (feature)"

git switch main
git merge feature-login   # Three-Way Merge, yeni merge commit yaranır
git log --oneline --graph --all   # vizual göstərir
```

---

<!-- Slide 5: Merge Conflict -->
## Merge Conflict — Ziddiyyətin Həlli

```
NƏDIR: İki branch EYNİ faylın EYNİ sətirini FƏRQLI şəkildə dəyişdirib.
Git özü qərar verə bilmir — sizdən qərar gözləyir.
```

```bash
# Konflikt yarandıqda Git faylı bu şəkildə işarələyir:
<<<<<<< HEAD
versiya = '2026'           # ← main branch-ındakı versiya
=======
versiya = '2027'           # ← feature branch-ındakı versiya
>>>>>>> feature-version

# ── HƏLLİN 5 ADDIMI ─────────────────────────────────────────────
# 1. git status — konfliktli faylları gör ("both modified")
git status

# 2. Faylı açıb <<<<<</======/>>>>>  işarələrini tap
nano version.py      # ya da VS Code, vim...

# 3. Qərar ver — hansını saxlayacaqsınız (ya da ikisini birləşdir)
# İşarələri SİLİN, faylı istədiyiniz kimi edin

# 4. Həll olunmuş faylı stage et
git add version.py

# 5. Merge-i tamamla
git commit           # mesaj avtomatik təklif olunur
```

<div class="warn">⚠️ `<<<<<<<`, `=======`, `>>>>>>>` işarələrini SİLMƏDƏN fayl commit etmək — kodun SINMASINA səbəb olur!</div>

---

<!-- Slide 6: Conflict Praktik Nümunə -->
## Merge Conflict — Praktik Nümunə

```bash
# ── Conflict qəsdən yaratmaq (öyrənmək üçün) ──────────────────

# main branch-da:
echo "VERSION = '2026'" > version.py
git commit -am "fix: versiya 2026 təyin edildi"

# feature branch-da (eyni sətri fərqli dəyişdir):
git switch -c feature-version --no-track -c --no-track 2>/dev/null || git switch -c feature-version main~1
echo "VERSION = '2027'" > version.py
git commit -am "fix: versiya 2027 təyin edildi"

# Merge etmə — KONFLIKT:
git switch main
git merge feature-version
# CONFLICT (content): Merge conflict in version.py

# Faylı açıb görmək:
cat version.py
# <<<<<<< HEAD
# VERSION = '2026'
# =======
# VERSION = '2027'
# >>>>>>> feature-version

# Qərar ver (2027-ni saxla):
echo "VERSION = '2027'" > version.py
git add version.py
git commit -m "merge: versiya konflikti həll edildi, 2027 seçildi"
```

<div class="r">Konflikt — fəlakət deyil, GÜNDƏLİK bir prosesdir. Sakit oxuyun, qərar verin, add + commit edin.</div>

---

<!-- Slide 7: Rebase -->
## `git rebase` — Tarixçəni "Düzləşdirmək"

<div class="bonus">🎁 BONUS mövzu — real işdə rast gəldikcə öyrənəcəksiniz, indi əzbərləməyə ehtiyac yoxdur</div>

```bash
# Rebase-dən ƏVVƏL:
main:    ●──●──●(C3)
              \
feature:       ●(C4)──●(C5)     ← feature C2-dən ayrılıb, main C3-ə çatıb

# git switch feature && git rebase main
# Rebase-dən SONRA:
main:    ●──●──●(C3)
                   \
feature:            ●(C4')──●(C5')   ← YENİ hash-larla, C3-ün üzərində
# C4 və C5 ARTIQ YOXDUR, C4' və C5' yarandı (eyni dəyişiklik, fərqli hash!)
```

```bash
git switch feature-login
git rebase main         # feature-login-i main-in üzərinə "köçürür"
git log --oneline --graph --all   # tarixçənin "düz xətt" olduğunu görün
```

**Merge vs Rebase:**
- **Merge** — 2 tarixçəni BİRLƏŞDİRİR (hər ikisini saxlayır, qovşaq commit)
- **Rebase** — commit-ləri YENİDƏN YAZIR (düz xətt, sanki əvvəldən başlayıb)

<div class="warn">⚠️ QIZIL QAYDA: Artıq PUSH edilmiş, başqaları ilə PAYLAŞILMIŞ branch-ı HEÇ VAXT rebase etməyin! Hash dəyişir → komanda yoldaşlarının tarixçəsi pozulur.</div>

---

<!-- Slide 8: Cherry-Pick -->
## `git cherry-pick` — Konkret Commit Köçürmək

<div class="bonus">🎁 BONUS mövzu — real işdə rast gəldikcə öyrənəcəksiniz, indi əzbərləməyə ehtiyac yoxdur</div>

```bash
# SSENARI: feature-A branch-ında bir bug-fix var (C7),
# amma bütün feature-A-nı merge etmək istəmirəm.
# Yalnız o BİR commit-i main-ə köçürmək istəyirəm.

feature-A:  ●──●(bug-fix, C7)──●   ← yalnız C7 lazımdır
main:       ●──●──● (HEAD)

git switch feature-A
git log --oneline          # C7-nin hash-ını tap, məs. d4e8a1

git switch main
git cherry-pick d4e8a1     # yalnız bu commit main-ə köçürülür

main:       ●──●──●──●(C7', eyni dəyişiklik, YENİ hash)
```

```bash
git cherry-pick <hash>         # tək commit
git cherry-pick <hash1> <hash2>  # bir neçə commit ardıcıl
git cherry-pick --abort        # konflikt varsa ləğv et
git cherry-pick --continue     # konflikt həll olduqdan sonra davam et
```

> **Analogy:** Cherry-pick = böyük tort qutusundan YALNIZ BİR dilim götürmək. Bütün tortu (branch-ı) gətirmirsən, sadəcə bir dilimi (commit-i) seçirsən.

---

<!-- Slide 9: Praktik Tapşırıq -->
## Praktik Tapşırıq — Sessiya 5

<div class="t">T.1 — Fast-Forward: feature branch yarat, commit et (main-i dəyişmə), main-ə keç, merge et. `git log --graph` ilə "straight line" gör.</div>

<div class="t">T.2 — Three-Way: HƏMDƏ main-i dəyişdir. Feature branch-ı da dəyişdir. Merge et. `git log --graph` ilə merge commit-in 2 valideynini gör.</div>

<div class="t">T.3 — Conflict: 2 branch-da EYNİ faylın EYNİ sətirini fərqli dəyişdir. Merge et. Konflikt faylını əl ilə oxu, qərar ver, həll et, commit et.</div>

<div class="r">Ən vacib tapşırıq: T.3 (conflict) — real komanda işindəki GÜNDƏLİK vəziyyətdir.</div>

<div class="tb">🎁 BONUS T.4 (könüllü) — Rebase: feature branch-ında 2 commit et. main-i dəyişdir. `git rebase main` ilə feature-i main-in üzərinə "köçür". `git log --graph` ilə fərqi gör.</div>

<div class="tb">🎁 BONUS T.5 (könüllü) — Cherry-pick: feature branch-ında 3 commit et. Yalnız ortadakı commit-i main-ə cherry-pick et.</div>

---

<!-- Slide 10: Ən Çox Edilən Səhvlər -->
## Ən Çox Edilən Səhvlər — Sessiya 5

<div class="warn">⚠️ Yanlış branch-dayken merge etmək — `git merge feature-login` o deməkdir ki, hazırda olduğunuz branch-A feature-login-i ALIR. ƏVVƏLCƏ `git status` ilə harada olduğunuzu yoxlayın.</div>

<div class="warn">⚠️ Merge conflict işarələrini `<<<<<<<`, `=======`, `>>>>>>>` SİLMƏDƏN commit etmək — proqram kodu sınır.</div>

<div class="warn">⚠️ Push edilmiş branch-ı rebase etmək — komanda yoldaşlarının tarixçəsi ilə TAM UYĞUNSUZLUQ yaranır.</div>

<div class="warn">⚠️ Cherry-pick-i "branch-ları merge etməyin asan yolu" kimi həddindən artıq istifadə etmək — tarixçədə eyni dəyişiklik 2 fərqli hash ilə görünür, sonra çaşqınlıq yaradır.</div>

---

<!-- Slide 11: Quiz -->
## Quiz — Sessiya 5

<div class="q">1. Fast-Forward Merge nə vaxt baş verir?</div>
<div class="a">Cari branch (məs. main), digər branch-ın (feature) tarixçəsinin birbaşa davamındadırsa — yəni main feature-dən ayrılandan sonra heç dəyişməyibsə.</div>

<div class="q">2. Three-Way Merge-də neçə "nöqtə" müqayisə olunur?</div>
<div class="a">3 nöqtə: (1) ortaq əcdad, (2) main-in son vəziyyəti, (3) feature-in son vəziyyəti. Merge commit-in 2 valideyni olur.</div>

<div class="q">3. Merge Conflict-i həll etmənin 5 addımını sayın.</div>
<div class="a">1) git status (konfliktli faylları gör) 2) Faylı aç, <<<<<<</=======>>>>>>>> tap 3) Qərar ver, işarələri sil 4) git add 5) git commit.</div>

<div class="q">4. (BONUS) Rebase merge-dən nə ilə fərqlənir?</div>
<div class="a">Merge iki tarixçəni BİRLƏŞDİRİR (yeni qovşaq commit). Rebase commit-ləri YENİDƏN YAZIR (yeni hash, düz xətt tarixçə).</div>

<div class="q">5. (BONUS) Cherry-pick nə vaxt faydalıdır?</div>
<div class="a">Bütün branch-ı deyil, konkret BİR commit-i başqa branch-a köçürmək lazım olduqda. Məs: production-da tək bug-fix, amma develop-da yarımçıq işlər var.</div>

> **Data Platform & Engineering Bootcamp · DPE-01** · Git/GitHub/GitLab Modulu
