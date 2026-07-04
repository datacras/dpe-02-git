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
  .step { background: #0F172A; color: #A7F3D0; padding: 3px 10px; border-radius: 4px; font-size: 0.75em; font-weight: 700; display: inline-block; margin-right: 6px; }
  .bonus { background: #F3E8FF; border-left: 4px solid #9333EA; padding: 4px 10px; border-radius: 0 6px 6px 0; margin: 3px 0; font-size: 0.76em; color: #581C87; font-weight: 600; }
  section::after { color: #848688; font-size: 0.72em; }
---

<!-- Slide 1: Title -->
# Git, GitHub & GitLab Bootcamp
## Sessiya 9 — Tam Praktiki Layihə

```
Bu sessiyada kursun BÜTÜN nəzəri biliyini BİR layihədə,
ADDIM-ADDIM tətbiq edirik.

10 Addım:  init → commit → branch → merge → conflict
           remote → clone → fetch/pull → stash → tag
```

> **Data Platform & Engineering Bootcamp · DPE-01**
> Git/GitHub/GitLab Modulu · Sessiya 9/9

---

<!-- Slide 2: Layihənin Strukturu -->
## Layihənin Strukturu

**student-project** — sadə Python tətbiqi

```
student-project/
├── README.md           # layihə sənədləşməsi
├── main.py             # əsas Python tətbiqi
├── requirements.txt    # asılılıqlar
└── .gitignore          # izlənməyəcək fayllar
```

**10 addımın xülasəsi:**

| Addım | Mövzu |
|-------|-------|
| 1 | Repository yarat, .gitignore, ilk strukturu qur |
| 2 | İlk commit (Conventional Commits formatında) |
| 3 | Feature branch yarat, dəyişiklik et |
| 4 | Fast-Forward Merge |
| 5 | Qəsdən Merge Conflict yarat və həll et |
| 6 | GitHub-da Remote repo yarat, push et |
| 7 | "Başqa kompüter" simulyasiyası — clone |
| 8 | Fetch + diff + pull iş axını |
| 9 | Stash iş axını |
| 10 | Annotated tag yarat, v1.0.0 relizi |

<div class="bonus">🎁 Qeyd: bu layihə qəsdən yalnız ƏSAS mövzulardan istifadə edir — rebase, cherry-pick, Detached HEAD, Git Flow bura daxil edilməyib. Onları iş həyatında lazım olanda öyrənəcəksiniz.</div>

---

<!-- Slide 3: Addım 1 — Repository Qurmaq -->
## Addım 1 — Repository Yaratmaq

```bash
mkdir student-project && cd student-project
git init

# .gitignore — İLK İŞ
cat > .gitignore << 'EOF'
__pycache__/
*.pyc
.env
venv/
*.log
.DS_Store
EOF

# README.md
cat > README.md << 'EOF'
# Student Project

Git/GitHub/GitLab kursunda istifadə olunan nümunə tələbə layihəsidir.

## Quraşdırma
pip install -r requirements.txt

## İşə salmaq
python main.py
EOF

# main.py
cat > main.py << 'EOF'
def greet(name):
    return f"Salam, {name}! Bootcamp-a xoş gəldin."

if __name__ == "__main__":
    print(greet("Tələbə"))
EOF

echo "requests==2.31.0" > requirements.txt

git status     # 4 fayl untracked görünməlidir
```

---

<!-- Vizual: 10 addımın xəritəsi -->
## Bu Sessiyada Nəyi Edəcəksiniz

```
  LOKAL İŞ  (addım 1-5):

  init → add → commit → branch → merge → conflict həll et

  REMOTE İŞ  (addım 6-8):

  remote add → push → clone → fetch → merge

  YANLI İŞ  (addım 9):

  stash → başqa branch → geri al (pop)

  TAKTİKA  (addım 10):

  tag → push tag → GitHub-da "Release" görün


  Nəticə nə olur?

    ● (HEAD, main, tag: v1.0.0, origin/main)
    ●   merge commit — README konflikti
    |\
    | ● feature/readme-update
    |/
    ● feat(greet): kurs adı əlavə edildi
    ● feat: ilkin layihə strukturu

  Bu tarixçəni SİZ yaradacaqsınız. Bu sessiyadan sonra
  Git-in bir developer-in gündəlik işini dəstəkləyən
  alət olduğunu artıq başa düşəcəksiniz, nəzəriyyə kimi yox.
```

---

<!-- Slide 4: Addım 2 + 3 -->
## Addım 2 — İlk Commit · Addım 3 — Branch

```bash
# ── ADDIM 2: İLK COMMİT ─────────────────────────────────────────
git add .
git status                    # hamısı staged
git commit -m "feat: ilkin layihə strukturu yaradıldı"
git log --oneline             # 1 commit

# ── ADDIM 3: FEATURE BRANCH ─────────────────────────────────────
git switch -c feature/greet-update

# main.py-ni yenilə:
cat > main.py << 'EOF'
def greet(name, course="DevOps Bootcamp"):
    return f"Salam, {name}! {course}-a xoş gəldin."

if __name__ == "__main__":
    print(greet("Tələbə"))
EOF

git add main.py
git commit -m "feat(greet): kurs adı parametri əlavə edildi"

git log --oneline --graph --all
# * a3f5e9c (HEAD -> feature/greet-update) feat(greet): kurs adı...
# * 7b2c4d1 (main) feat: ilkin layihə strukturu yaradıldı
```

---

<!-- Slide 5: Addım 4 + 5 -->
## Addım 4 — Fast-Forward Merge · Addım 5 — Conflict

```bash
# ── ADDIM 4: FAST-FORWARD MERGE ─────────────────────────────────
git switch main               # main bu vaxt dəyişməyib
git merge feature/greet-update   # Fast-Forward!
git log --oneline             # merge commit YOXDUR
git branch -d feature/greet-update

# ── ADDIM 5: QƏSDƏN KONFLİKT YARATMAQ ──────────────────────────
# main-də README-nin ilk sətirini dəyiş:
sed -i '' '1s/.*/# Student Project — RƏSMİ Git\/GitHub\/GitLab Kursu/' README.md
git commit -am "docs: README başlığı yeniləndi (main-də)"

# Köhnə nöqtədən ayrılıb EYNİ sətri fərqli dəyiş:
git switch -c feature/readme-update $(git log --oneline | tail -1 | cut -d' ' -f1)
sed -i '' '1s/.*/# Student Project — NÜMUNƏ Git\/GitHub\/GitLab Kursu/' README.md
git commit -am "docs: README başlığı fərqli redaktə edildi (feature-də)"

# Merge → KONFLİKT!
git switch main
git merge feature/readme-update
# CONFLICT (content): Merge conflict in README.md
cat README.md   # <<<<<<< HEAD, =======, >>>>>>> görün

# Həll: istədiyiniz versiyayı saxlayın, işarələri silin
echo "# Student Project — RƏSMİ VƏ NÜMUNƏ Git/GitHub/GitLab Kursu" > README.md
cat README.md >> README.md   # qalan məzmunu əl ilə bərpa edin
git add README.md && git commit -m "merge: README konflikti həll edildi"
```

---

<!-- Slide 6: Addım 6 + 7 -->
## Addım 6 — GitHub Push · Addım 7 — Clone

```bash
# ── ADDIM 6: GITHUB-A PUSH ──────────────────────────────────────
# GitHub-da: yeni repo yarat "student-project" (README olmadan!)

git remote add origin https://github.com/SİZİN-ADİNİZ/student-project.git
git remote -v                 # origin görünür

git push -u origin main       # ilk push, upstream qeyd edir
# GitHub-da repo-ya baxın — bütün tarixçə orada!

# ── ADDIM 7: "BAŞQA KOMPÜTER" SIMULYASIYASI ─────────────────────
cd ..
git clone https://github.com/SİZİN-ADİNİZ/student-project.git student-project-clone
cd student-project-clone

git log --oneline             # bütün tarixçə artıq burada!
git remote -v                 # origin avtomatik qeyd olunub
ls -la                        # bütün fayllar mövcuddur

# Müəllim üçün qeyd: Bu addım real həyatda "komandaya yeni qoşulan tələbə"
# ssenarisini simulyasiya edir — onlar sadəcə git clone edir, HƏR ŞEY (bütün
# tarixçə) ANİNDA kompüterlərinə köçür.
```

---

<!-- Slide 7: Addım 8 + 9 -->
## Addım 8 — Fetch/Pull · Addım 9 — Stash

```bash
# ── ADDIM 8: FETCH + PULL ───────────────────────────────────────
# Orijinal qovluqda dəyişiklik et + push et:
cd ../student-project
echo "## Töhfə vermək\nPull Request açın." >> README.md
git commit -am "docs: töhfə bölməsi əlavə edildi"
git push origin main

# Klon qovluğunda fetch + merge:
cd ../student-project-clone
git fetch origin              # yenilik var, amma local main TOXUNULMUR
git log origin/main --oneline -1   # remote-da nə olduğunu gör
git diff main origin/main          # fərqi yoxla
git merge origin/main              # əl ilə, nəzarətli birləşdir
cat README.md                      # yenilik gəldi

# ── ADDIM 9: STASH ──────────────────────────────────────────────
cd ../student-project
echo "# yarımçıq qeyd" >> main.py   # commit etmə!
git status                # modified: main.py

git stash                 # yarımçıq işi anbara qoy
git status                # working tree clean

git switch -c hotfix/typo
echo "# düzəliş edildi" >> requirements.txt
git commit -am "fix: requirements.txt-də kiçik düzəliş"
git switch main && git merge hotfix/typo && git branch -d hotfix/typo

git stash pop             # yarımçıq işimiz geri gəldi!
cat main.py               # qeydin geri gəldiyini görün
git restore main.py       # demo məqsədli dəyişikliyi ləğv et
```

---

<!-- Slide 8: Addım 10 — Tag + Yekun -->
## Addım 10 — Tag Yaratmaq

```bash
# ── ADDIM 10: ANNOTATED TAG ─────────────────────────────────────
git log --oneline             # son commit-i seçin

git tag -a v1.0.0 -m "İlk stabil reliz: tələbə layihəsi əsası hazırdır"
git tag                       # tag-ı görün
git show v1.0.0               # detallarına baxın

git push origin v1.0.0        # tag-ı remote-a göndərin
# GitHub-da "Releases" bölməsini yoxlayın

# ── YEKİN YOXLAMA ────────────────────────────────────────────────
git log --oneline --graph --all
# Görməli olduqlarınız:
# * (HEAD -> main, tag: v1.0.0, origin/main) merge: README konflikti...
# *   merge: feature/readme-update...
# |\
# | * feat(greet): kurs adı parametri...
# |/
# * feat: ilkin layihə strukturu...

git tag                       # v1.0.0
git remote -v                 # origin GitHub-a qoşulub
git log --oneline | wc -l     # neçə commit?
```

<div class="r">Təbrik! Bu addımları icra edərək siz artıq REAL bir komandanın gündəlik işlədiyini bütün Git əməliyyatlarını öz əlinizlə tamamladınız.</div>

---

<!-- Slide 9: Tam Workflow Xülasəsi -->
## Tam Workflow — Öyrəndiklərimizin Xülasəsi

```
Sessiya 1: git init  ·  git config  ·  3-zone  ·  SHA
Sessiya 2: git status  ·  git add  ·  .gitignore  ·  git commit
Sessiya 3: git log  ·  git diff  ·  git restore  ·  git reset
Sessiya 4: git branch  ·  HEAD  ·  Detached HEAD  ·  stash  ·  tag
Sessiya 5: merge (FF + 3-way)  ·  conflict  ·  rebase  ·  cherry-pick
Sessiya 6: remote  ·  clone  ·  fetch vs pull  ·  push  ·  fork
Sessiya 7: PR/MR  ·  GitHub  ·  GitLab  ·  CI/CD giriş
Sessiya 8: Git Flow  ·  GitHub Flow  ·  TBD  ·  Conventional Commits
Sessiya 9: Tam praktiki layihə (10 addım)

── GÜNDƏLİK İŞ AXINI (hər zaman) ──────────────────────────────
git switch main && git pull origin main     # son vəziyyəti al
git switch -c feature/yeni-is              # branch yarat
# ... kiçik, fokuslanmış commit-lər ...
git push -u origin feature/yeni-is         # push et
# PR/MR aç → review → merge
git switch main && git pull                # merge-dən sonra yenilə
git branch -d feature/yeni-is             # köhnə branch-ı sil
```

---

<!-- Slide 10: Müsahibə Hazırlığı -->
## Müsahibə Hazırlığı — Ən Çox Soruşulan Suallar

```
KONSEPTUAL:
  Git VCS-in hansı kateqoriyasınadır? → Distributed
  3-zone arxitekturanı izah et        → WD → Staging → Repository
  SHA hash niyə vacibdir?             → bütövlük + deduplication

ƏMƏL:
  git fetch vs git pull fərqi?        → fetch = al, birləşdirmə; pull = al + birləşdir
  Merge Conflict necə həll edilir?    → status → fayl aç → qərar ver → add → commit
  Rebase nə vaxt təhlükəlidir?        → push edilmiş branch üzərində

PLATFORM:
  GitHub vs GitLab fərqi?             → GitHub: açıq mənbə icması; GitLab: tam DevOps
  PR vs MR nədir?                     → eyni konsepsiya, fərqli adlar (GitHub/GitLab)
  Fork vs Clone?                      → Fork: platform kopyası; Clone: lokal kopyası

WORKFLOW:
  Git Flow-da neçə branch növü var?   → 5 (main, develop, feature, release, hotfix)
  GitHub Flow-un əsas prinsipi?       → main HƏMİŞƏ deploy oluna bilən
  Conventional Commits formatı?       → feat(scope): qısa təsvir
```

> **Data Platform & Engineering Bootcamp · DPE-01** · Git/GitHub/GitLab Modulu · Sessiya 9/9
> Bütün 9 sessiya tamamlandı!
