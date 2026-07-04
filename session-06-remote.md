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
## Sessiya 6 — Remote Əməkdaşlıq

```
Mövzular:  Remote · Origin · Upstream
           git clone  ·  git fetch vs pull  ·  git push  ·  Fork
```

> **Data Platform & Engineering Bootcamp · DPE-01**
> Git/GitHub/GitLab Modulu · Sessiya 6/9

---

<!-- Slide 2: Remote, Origin, Upstream -->
## Remote, Origin, Upstream — 3 Termin

```
[upstream]   ← orijinal repo (başqasının layihəsi, məs. django/django)
     ↑
     │ nadir hallarda push, adətən yalnız pull/fetch
     │
[origin]     ← SİZİN fork-unuz (GitHub-da, sizin hesabınızda)
    ↑↓
  (push/pull)
     │
[Local Repository]  ← sizin kompüterinizdə

# .git/config içində:
[remote "origin"]
    url = https://github.com/SIZINAD/repo.git
[remote "upstream"]
    url = https://github.com/ORİJİNAL-SAHİB/repo.git
```

- **Remote** — ümumi termin: başqa yerdəki (server/kompüter) istənilən repo
- **Origin** — standart, ən çox istifadə olunan ad; klon etdiyiniz mənbə avtomatik **origin** adlanır
- **Upstream** — Fork ssenarisində: orijinal mənbə layihəsi

> Bir repo-nun BİRDƏN ÇOX remote-u ola bilər. "origin" məcburi deyil, sadəcə konvensiyadır.

---

<!-- Slide 3: git clone -->
## `git clone` — Remote Repo-nu Kopyalamaq

```bash
# ── Əsas ──────────────────────────────────────────────────────
git clone https://github.com/username/student-project.git
# Nəticə: student-project/ qovluğu yaranır
#         .git/ daxilindədir
#         "origin" remote avtomatik qeyd olunur
#         default branch (main) checkout edilir

cd student-project
git remote -v                  # origin URL-i göstərir
git log --oneline              # bütün tarixçə artıq lokaldadır

# ── Variantlar ─────────────────────────────────────────────────
git clone <url> yeni-qovluq-adi      # fərqli qovluq adı
git clone --depth 1 <url>            # yalnız son commit (böyük repolar üçün)
git clone -b feature-login <url>     # konkret branch-ı checkout et

# ── Clone nə edir? (5 addım) ───────────────────────────────────
# 1. Yeni lokal qovluq yaradır
# 2. .git/ strukturunu qurur
# 3. Bütün obyektləri (blob, tree, commit) local .git/objects-a köçürür
# 4. Bütün branch-ları refs/remotes/origin/* altında yaradır
# 5. Default branch-ı (main) lokal olaraq checkout edir
```

<div class="r">Clone = kitabxanadakı kitabın TAM FOTOKOPİYASINI çıxarmaq — yalnız son səhifəni deyil, bütün tarixi ilə birlikdə.</div>

---

<!-- Vizual: Remote axını -->
## Telefon → Cloud, Amma Kod

```
  [Sənin kompüterin]                  [GitHub / GitLab]
  ──────────────────                   ────────────────
                         ──push──►
       local repo                        remote repo
                         ◄──pull──       (origin)


  git remote add origin <url>   ← "bu ünvan = origin adını daşısın"
  git push -u origin main       ← "göndər, artıq bu branch-ı yadda saxla"
  git pull origin main          ← "al, avtomatik birləşdir"

  Komanda yoldaşında (ya başqa kompüterdə):
  git clone <url>               ← bütün tarixçəni BİR əmrlə alır


  FETCH vs PULL — bu fərqi bil:

  fetch = "orada nə var, bax"   (sən hərəkətsiz qalırsan)
  pull  = fetch + avtomatik birləşdir

  Tövsiyə olunan:
  git fetch → git diff main origin/main → git merge
  Bu sıra NƏZARƏT verir. git pull daha sürətli, amma "gözlərini yum".
```

---

<!-- Slide 4: git fetch vs git pull -->
## `git fetch` vs `git pull` — KRİTİK FƏRQ

<div class="bonus">🎁 BONUS: gündəlik iş üçün sadə `git pull` kifayətdir — bu slide daha çox NƏZARƏT istəyənlər üçündür</div>

```bash
# ── git fetch ──────────────────────────────────────────────────
git fetch origin
# Remote-dakı YENİLİKLƏRİ yükləyir (refs/remotes/origin/* yeniləyir)
# Sizin LOCAL main branch-ınıza HEÇ TOXUNMUR
# "Uzaqda nə var" məlumatını alır, birləşdirmir

git log origin/main --oneline -3   # remote-da nə olduğunu gör
git diff main origin/main           # fərqi yoxla
git merge origin/main               # əl ilə, NƏZARƏTLİ birləşdir

# ── git pull ───────────────────────────────────────────────────
git pull origin main
# = git fetch origin + git merge origin/main
# Dərhal birləşdirir, nəzarətsiz

git pull --rebase origin main
# = git fetch + git rebase (daha "təmiz" tarixçə üçün)
```

```
FETCH:  [Remote] ──fetch──► refs/remotes/origin/main   local main TOXUNULMUR
PULL:   [Remote] ──fetch──► refs/remotes/origin/main
                                    │
                             avtomatik merge
                                    ▼
                             refs/heads/main (BİRLƏŞDİRİLİR)
```

<div class="warn">⚠️ Həmişə düşünmədən `git pull` — gözlənilməz merge commit-lər və ya konfliktlər yarana bilər. Fetch → diff → merge tövsiyə olunur.</div>

---

<!-- Slide 5: git push -->
## `git push` — Local Commit-ləri Remote-a Göndərmək

```bash
# ── Əsas ──────────────────────────────────────────────────────
git push origin main           # local main-i origin-dəki main-ə göndər

git push -u origin main        # ilk dəfə: upstream qeyd et
                               # sonra sadəcə `git push` kifayət edir
                               # (-u = --set-upstream)

# ── Rədd olunma — "rejected" ──────────────────────────────────
git push origin main
# error: failed to push some refs
# hint: Updates were rejected because the remote contains work

# Remote-da sizdə olmayan yeni commit-lər var!
# HƏLL:
git pull origin main           # əvvəlcə AL
git push origin main           # sonra GÖNDƏR

# ── Güclü (təhlükəli) push ─────────────────────────────────────
git push --force               # MƏCBUR ƏVƏZ EDIR — başqalarının işini silə bilər!
git push --force-with-lease    # daha TƏHLÜKƏSİZ: yalnız SİZİN gözlədiyin vəziyyət varsa force edir
```

<div class="bonus">🎁 BONUS: `--force` — gündəlik işdə lazım olmur, adını tanımaq kifayətdir</div>

<div class="warn">⚠️ `git push --force` shared branch-larda — komanda yoldaşlarının işini SİLƏ BİLƏR. `--force-with-lease` istifadə edin.</div>

---

<!-- Slide 6: Push Workflow -->
## Gündəlik Push Workflow

```bash
# ── Düzgün ardıcıllıq ─────────────────────────────────────────

# 1. Hər zaman ən son vəziyyəti alın
git pull origin main           # və ya fetch + diff + merge

# 2. Öz branch-ınızda işləyin
git switch -c feature/yeni-funksiya

# 3. Kiçik, fokuslanmış commit-lər edin
git add main.py
git commit -m "feat(api): yeni endpoint əlavə edildi"

# 4. Remote-a göndərin
git push -u origin feature/yeni-funksiya

# 5. PR/MR açın (növbəti sessiya)

# ── Tez-tez görünən xəbərdarlıq ─────────────────────────────
# "Your branch is ahead of 'origin/main' by 2 commits."
# → Lokal commit-ləriniz hələ push edilməyib. `git push` edin.

# "Your branch is behind 'origin/main' by 3 commits."
# → Remote-da yeni commit-lər var. `git pull` edin.
```

<div class="r">Qayda: Push etməzdən ƏVVƏL pull et, konfliktləri ERKƏN aşkarla.</div>

---

<!-- Slide 7: Fork Workflow -->
## Fork — Başqasının Layihəsinə Töhfə Vermək

```
[upstream: django/django]   ← yazma icazəniz yoxdur
        │
        │ Fork (GitHub/GitLab düyməsi)
        ▼
[origin: SİZİNAD/django]    ← TAM nüsxə, SİZİN hesabınızda
        │
        │ git clone
        ▼
[Local Repository]          ← öz kompüterinizdə işləyirsiniz
        │
        │ git push
        ▼
[SİZİNAD/django yenilənir]
        │
        │ Pull Request (orijinala təklif)
        ▼
[django/django sahibi nəzərdən keçirir, qəbul/rədd edir]
```

```bash
# Fork etdikdən sonra lokal:
git clone https://github.com/SIZINAD/django.git
cd django
git remote add upstream https://github.com/django/django.git
git remote -v                  # həm origin, həm upstream görünür

# Upstream-dən yenilikləri almaq:
git fetch upstream
git merge upstream/main
git push origin main           # fork-unuzu yeniləyin
```

---

<!-- Slide 8: SSH Açarı Qurulumu -->
## SSH Autentifikasiyası — Bir Dəfəlik Qurulum

```bash
# ── SSH açarı yaratmaq ────────────────────────────────────────
ssh-keygen -t ed25519 -C "email@example.com"
# Fayllar: ~/.ssh/id_ed25519 (private) + ~/.ssh/id_ed25519.pub (public)

# ── Public açarı GitHub/GitLab-a əlavə etmək ─────────────────
cat ~/.ssh/id_ed25519.pub      # bu məzmunu kopyalayın

# GitHub: Settings → SSH and GPG keys → New SSH key
# GitLab: Settings → SSH Keys → Add new key

# ── Əlaqəni test etmək ───────────────────────────────────────
ssh -T git@github.com
# "Hi username! You've successfully authenticated..."

ssh -T git@gitlab.com
# "Welcome to GitLab, @username!"

# ── SSH URL istifadəsi ────────────────────────────────────────
git clone git@github.com:username/repo.git    # SSH
git clone https://github.com/username/repo.git  # HTTPS (şifrə/token lazım)

# Mövcud remote-u SSH-ə çevirmək:
git remote set-url origin git@github.com:username/repo.git
```

<div class="r">SSH = bir dəfə qurulur, həmişə şifrəsiz işləyir. HTTPS-də GitHub Personal Access Token (PAT) lazımdır.</div>

---

<!-- Slide 9: Praktik Tapşırıq -->
## Praktik Tapşırıq — Sessiya 6

<div class="t">T.1 — GitHub-da boş, README-siz yeni "student-project" repo yaradın. Local student-project-i `git remote add origin <url>` + `git push -u origin main` ilə göndərin.</div>

<div class="t">T.2 — `git clone` ilə repo-nu BAŞQA bir qovluğa klon edin (simulyasiya: "başqa kompüter"). `git log --oneline` ilə tarixçənin tam kopyalandığını görün.</div>

<div class="t">T.3 — Orijinal qovluqda dəyişiklik edib push edin. Klon qovluğunda `git fetch origin` + `git log origin/main --oneline` + `git merge origin/main` ilə yenilikləri alın. (Sadə alternativ: birbaşa `git pull origin main`.)</div>

<div class="t">T.4 — SSH açarı yaradın, GitHub-a əlavə edin, `ssh -T git@github.com` ilə test edin.</div>

```bash
# Yoxlama:
git remote -v                  # origin URL düzgün görünür?
git log --oneline              # klon tarixçəsi tam?
git status                     # working tree clean?
```

<div class="r">T.3 ən vacibdir — `git pull` əvəzinə `fetch + diff + merge` axınını öyrənin.</div>

---

<!-- Slide 10: Ən Çox Edilən Səhvlər -->
## Ən Çox Edilən Səhvlər — Sessiya 6

<div class="warn">⚠️ Remote-da yeni commit-lər varkən pull etmədən push etmək — "rejected, non-fast-forward" xətası. Həll: əvvəlcə `git pull`, sonra `git push`.</div>

<div class="warn">⚠️ Fork etdikdən sonra upstream əlavə etməyi unutmaq — fork-unuz orijinal repo-dan GERİ QALIR, yenilikləri ala bilmirsiniz.</div>

<div class="warn">⚠️ "Origin" sözünü "GitHub-un özü" zənn etmək — origin sadəcə BİR ADdır (konvensiya), başqa ad da qoya bilərsiniz.</div>

<div class="warn">⚠️ Mövcud Git repo-nun İÇİNDƏ klonlamaq — "repo içində repo" yaranır. Həll: əvvəlcə `cd ..`, sonra clone.</div>

<div class="warn">⚠️ `git push --force` shared branch-da — komanda yoldaşlarının işini silə bilər. `--force-with-lease` istifadə edin.</div>

---

<!-- Slide 11: Quiz -->
## Quiz — Sessiya 6

<div class="q">1. "Remote", "Origin" və "Upstream" arasındakı fərq nədir?</div>
<div class="a">Remote — ümumi kateqoriya. Origin — ən çox istifadə olunan ad (klon mənbəsi). Upstream — fork ssenarisində orijinal layihə sahibinin repo-su.</div>

<div class="q">2. `git fetch` local branch-ı dəyişdirmi?</div>
<div class="a">Xeyr — fetch yalnız refs/remotes/origin/* yeniləyir. Lokal main TOXUNULMUR. Pull = fetch + merge.</div>

<div class="q">3. `git push` "rejected" verərsə nə etmək lazımdır?</div>
<div class="a">Remote-da sizdə olmayan commit-lər var. Əvvəlcə `git pull origin main` (fetch + merge), sonra yenidən `git push`.</div>

<div class="q">4. Fork və Clone arasındakı fərq nədir?</div>
<parameter name="content">
<div class="a">Fork — PLATFORMA səviyyəsindədir (GitHub/GitLab-da), başqa hesabda yeni repo yaranır. Clone — LOKAL kompüterə kopyalamadır. Fork → Clone → iş axını.</div>

<div class="q">5. SSH vs HTTPS autentifikasiyası fərqi?</div>
<div class="a">SSH: bir dəfə açar yaradılır, şifrəsiz işləyir. HTTPS: hər push-da GitHub PAT (Personal Access Token) lazımdır. SSH tövsiyə olunur.</div>

> **Data Platform & Engineering Bootcamp · DPE-01** · Git/GitHub/GitLab Modulu
