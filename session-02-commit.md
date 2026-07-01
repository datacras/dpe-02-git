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
## Sessiya 2 — İzləmə və Commit

```
Mövzular:  git status  ·  git add  ·  .gitignore
           git commit  ·  --amend
```

> **Data Platform & Engineering Bootcamp · DPE-01**
> Git/GitHub/GitLab Modulu · Sessiya 2/9

---

<!-- Slide 2: Xatırlatma -->
## Xatırlatma — 3-Zone Arxitektura

```
┌──────────────────┐  git add   ┌─────────────────┐ git commit ┌─────────────┐
│  Working         │ ─────────► │  Staging Area   │ ─────────► │ Repository  │
│  Directory       │            │  (Index)        │            │  (.git/)    │
│                  │ ◄───────── │                 │            │             │
│  real fayllar    │ git restore│ commit üçün     │            │  SHA commit │
│  dəyişiklik edir │            │ seçilmişlər     │            │  daimi tarix│
└──────────────────┘            └─────────────────┘            └─────────────┘
```

**Bu sessiya:** Staging → Repository axınını tam öyrənəcəksiniz.

- `git status` — Hər şey hansı zonadadır?
- `git add` — Working Dir → Staging
- `.gitignore` — Git-ə nəyi İZLƏMƏ deyirik
- `git commit` — Staging → Repository
- `git commit --amend` — Son commit-i düzəlt

---

<!-- Slide 3: git status -->
## `git status` — Vəziyyəti Oxumaq

```bash
$ git status

On branch main

Changes to be committed:        # ← STAGED (Staging Area-dadır)
  (use "git restore --staged <file>..." to unstage)
        new file:   main.py     # ← git add edilib, commit gözləyir

Changes not staged for commit:  # ← MODIFIED (Working Dir-dədir)
  (use "git add <file>..." to update what will be committed)
        modified:   README.md   # ← dəyişdirildi, amma add edilmədi

Untracked files:                # ← GİT BİLMİR (heç vaxt add edilməyib)
  (use "git add <file>..." to include in what will be committed)
        config.json             # ← yeni fayl, hələ izlənmir

$ git status -s                 # ← qısa format
M  README.md   # M = modified, 1-ci sütun=staged, 2-ci=unstaged
A  main.py     # A = added (staged)
?? config.json # ?? = untracked
```

<div class="r">git status ─ "nə edəcəyimi bilmirəm" deyəndə İLK işə salacağınız əmr.</div>

---

<!-- Vizual: 3-zona sadə dildə -->
## Supermarket kimi düşün

```
  ──────────────────────────────────────────────────────────
  RƏFLƏR               KONVEYER LENTİ         KASSA
  (working directory)   (staging area)         (repository)
  ──────────────────────────────────────────────────────────

  [login.py]      →
  [main.py]       →    [login.py]     →    ● commit yarandı
  [style.css]          [style.css]         "feat: login əlavə edildi"

  git add login.py style.css    ←  rəfdən götür, lentə qoy
  git add .                     ←  rəfdəki HAMI şeyi lentə qoy
  git commit -m "..."           ←  kassa vur, "çek" çıxır

  Lentdəkiləri geri çıxarmaq istəyirsən?
  → git restore --staged <fayl>    (commit olmamışdan qabaq)

  Kassa vurduqdan sonra pişman oldun?
  → git reset --soft HEAD~1        (commit yox olur, fayllar qalır)
```

> Staging area-nın nöqtəsi: hər şeyi yox, **seçdiklərini** commit et.
> `git add -p` ilə faylın içindən hissə-hissə seçmək olar. Peşəkarcasına.

---

<!-- Slide 4: git add -->
## `git add` — Staging Area-ya Göndərmək

```bash
# ── Əsas istifadə ──────────────────────────────────────────────
git add main.py              # konkret fayl
git add src/                 # bütün qovluq
git add *.py                 # pattern ilə
git add .                    # cari qovluqdakı HƏR ŞEY

# ── Güclü variantlar ───────────────────────────────────────────
git add -p main.py           # ← interactive: hunk-hunk seçim
                             #   y=bəli, n=xeyr, s=böl, q=çıx

git add -A                   # ← həm dəyişiklik, həm silinmə, həm yeni
                             # (köhnə Git-də add . silinmələri əhatə etmirdi)
```

**`git add -p` niyə vacibdir?**
```
@@ -1,3 +1,5 @@
 def login():
+    log("giriş edildi")   # ← bu hissəni STAGE ET (y)
     return True

+def logout():              # ← bu hissəni STAGE ETMƏ (n)
+    pass
```

> Bir faylda 2 ayrı iş var → `-p` ilə yalnız lazımı hissəni seçin → 2 ayrı commit yapın

<div class="warn">⚠️ `git add .` hər şeyi stage edir — həssas faylları (şifrə, .env) da daxil olmaqla!</div>

---

<!-- Slide 5: .gitignore -->
## `.gitignore` — İzləməməli Fayllar

```bash
# .gitignore faylının məzmunu — repo kökündə yaradılır:

# Python
__pycache__/
*.pyc
*.pyo
*.egg-info/

# Virtual mühit
venv/
.env/

# Həssas məlumatlar (ƏN VACİB!)
.env
*.key
secrets.json
config/local.py

# OS faylları
.DS_Store         # macOS
Thumbs.db         # Windows

# IDE
.vscode/
.idea/

# Build məhsulları
dist/
build/
*.log
```

```bash
git status          # .gitignore-dakı fayllar artıq görünmür
git check-ignore -v .env   # niyə ignore edilir?
```

<div class="warn">⚠️ Artıq commit olunmuş fayl .gitignore-a əlavə etsəniz, GİT HƏLƏ İZLƏYİR! → `git rm --cached fayl` lazımdır.</div>

---

<!-- Slide 6: .gitignore Şablonları -->
## `.gitignore` — Praktik Nüanslar

```bash
# PATTERN sintaksisi:
*.log           # bütün .log faylları
!important.log  # istisna: important.log izlənsin
logs/           # logs qovluğu (trailing /)
**/temp         # istənilən dərinlikdə temp qovluğu
doc/**/*.pdf    # doc/ içindəki bütün pdf

# ── ÇOX VACİB QAYDA ──────────────────────────────────────────
# Artıq izlənən fayl .gitignore-a əlavə etsəniz:
echo ".env" >> .gitignore
git rm --cached .env          # ← index-dən çıxar (disk-dən SİLMƏZ)
git commit -m "fix: .env izlənmədən çıxarıldı"

# ── .gitignore üçün hazır şablonlar ─────────────────────────
# github.com/github/gitignore — Python, Node, Java, Go...

# ── Lokal istisna (repo-ya commit etmədən) ───────────────────
# .git/info/exclude faylına əlavə edin
echo "*.todo" >> .git/info/exclude
```

<div class="r">Yaxşı vərdiş: `git init`-dən dərhal sonra İLK iş — .gitignore yaratmaq. Sonradan düzəltmək çətindir.</div>

---

<!-- Slide 7: git commit -->
## `git commit` — Dəyişikliyi Qeyd Etmək

```bash
# ── Əsas istifadə ──────────────────────────────────────────────
git commit -m "feat: login funksiyası əlavə edildi"
# ← -m = mesaj, birbaşa verilir

git commit                   # ← $EDITOR açılır, ətraflı mesaj yaz
                             #   (Vim: i yazma, :wq saxla/çıx)

# ── Tez-tez istifadə ───────────────────────────────────────────
git commit -am "fix: null pointer xətası düzəldildi"
# ← -a: artıq izlənən faylları avtomatik stage edir (yeni fayllar yox!)
# ← -m: mesaj birlikdə

# ── Son commit-i düzəlt ────────────────────────────────────────
git commit --amend -m "feat: giriş funksiyası əlavə edildi"
# ← Mesajı dəyişir VƏ staged dəyişiklikləri əlavə edir
# ← Yeni commit YARATMIR — köhnəni ƏVƏZ EDIR (hash dəyişir!)
```

**`git commit -am` xəbərdarlığı:**
```bash
echo "print('yeni fayl')" > yeni.py   # yeni, untracked
git commit -am "test"                  # yeni.py-ni ƏHATƏ ETMİR
# Həll: git add yeni.py + git commit -m "..."
```

<div class="warn">⚠️ `--amend` artıq push edilmiş commit-ə tətbiq etməyin — başqalarının tarixçəsini pozur!</div>

---

<!-- Slide 8: Yaxşı Commit Mesajı -->
## Yaxşı Commit Mesajı Necə Yazılır?

```
PİS mesajlar:          YAXŞI mesajlar (Conventional Commits):

"fix"                  "fix(auth): şifrə sıfırlama e-poçtu göndərilmirdi"
"update"               "feat(search): istifadəçi axtarış filtri əlavə edildi"
"asdasd"               "docs(readme): API açarı qurulumu izahı əlavə edildi"
"dəyişdim"             "refactor(db): sorğu funksiyaları optimallaşdırıldı"
"wip"                  "chore: .gitignore-a __pycache__ əlavə edildi"
```

**Conventional Commits formatı:**
```
<tip>(<əhatə>): <qısa təsvir>

[ətraflı izah — könüllü]

[footer — BREAKING CHANGE: ... — könüllü]
```

**Əsas tiplər:**
- `feat` — yeni funksiya
- `fix` — bug düzəlişi
- `docs` — sənədləşmə
- `refactor` — kod yenidənstrukturlaşdırma
- `chore` — build/alət konfigurasiyası
- `test` — test əlavəsi/düzəlişi

---

<!-- Slide 9: Tam Workflow -->
## Tam Workflow — Add → Commit Tsikli

```bash
# ── İlk dəfə (Sessiya 1-dən davam) ────────────────────────────
cd student-project   # git init + config artıq edilib

# ── .gitignore yarat ───────────────────────────────────────────
cat > .gitignore << 'EOF'
__pycache__/
*.pyc
.env
*.log
.DS_Store
EOF

# ── Layihə faylları yarat ──────────────────────────────────────
echo "# Student Project\nGit Bootcamp nümunə layihəsi." > README.md
cat > main.py << 'EOF'
def greet(name):
    return f"Salam, {name}! Bootcamp-a xoş gəldin."

if __name__ == "__main__":
    print(greet("Tələbə"))
EOF

# ── Status → Add → Commit ──────────────────────────────────────
git status                            # 3 fayl untracked
git add .                             # hamısını stage et
git status                            # hamısı "Changes to be committed"
git commit -m "feat: ilk layihə strukturu yaradıldı"
git log --oneline                     # 1 commit görürsünüz
```

<div class="r">Nəticə: İlk commit tamamlandı. git log ilə tarixçəni görün.</div>

---

<!-- Slide 10: Praktik Tapşırıq -->
## Praktik Tapşırıq — Sessiya 2

<div class="t">T.1 — .gitignore yarat, __pycache__/ və .env əlavə et, git status ilə yoxla</div>

<div class="t">T.2 — 3 fayl yarat: README.md, main.py, config.json. Yalnız README.md və main.py-ni stage et. git status -s ilə fərqi gör.</div>

<div class="t">T.3 — git commit -m "feat: ilk fayllar" ilə commit et. Sonra README.md-ə bir sətir əlavə edib git commit --amend -m "feat: README və main.py əlavə edildi" et.</div>

<div class="t">T.4 — main.py-ə 2 fərqli funksiya əlavə et. git add -p ilə yalnız BİRİNİ stage et, commit et. Sonra ikincisini ayrıca commit et.</div>

```bash
# Nəticəni yoxlamaq üçün:
git log --oneline          # 3 commit görünməlidir (T.2+T.3, T.4a, T.4b)
git status                 # working tree clean olmalıdır
```

<div class="warn">⚠️ T.4 ən vacib tapşırıqdır — git add -p real iş həyatında çox istifadə olunur.</div>

---

<!-- Slide 11: Ən Çox Edilən Səhvlər -->
## Ən Çox Edilən Səhvlər — Sessiya 2

<div class="warn">⚠️ `git add .` ilə .env, şifrə, API açarı commit etmək — bu məlumatlar tarixçədə QALIR, hətta silsəniz belə. .gitignore-u ƏN ƏVVƏL yaradın!</div>

<div class="warn">⚠️ `git commit -am` yeni (untracked) faylları stage etmir — yalnız artıq izlənən fayllar üçün işləyir.</div>

<div class="warn">⚠️ `--amend` push edilmiş commit-ə — başqalarının tarixçəsi ilə uyğunsuzluq yaranır (hash dəyişir).</div>

<div class="warn">⚠️ Artıq commit olunmuş fayl .gitignore-a əlavə etmək — Git hələ izləyir! `git rm --cached fayl` lazımdır.</div>

<div class="warn">⚠️ Mənasız commit mesajı yazmaq — 6 ay sonra "fix" yazılmış commit-in NƏ etdiyini anlaya bilməzsiniz.</div>

---

<!-- Slide 12: Quiz -->
## Quiz — Sessiya 2

<div class="q">1. `git status` çıxışında "Changes not staged for commit" nə deməkdir?</div>
<div class="a">Fayl dəyişdirilib (Working Directory-dədir) amma hələ `git add` edilməyib — Staging Area-da yoxdur.</div>

<div class="q">2. `git add .` vs `git add -p` fərqi nədir?</div>
<div class="a">add . — hər şeyi stage edir. add -p — interaktiv, hunk-hunk seçim imkanı verir. Daha dəqiq commit üçün -p tövsiyə olunur.</div>

<div class="q">3. .gitignore-a əlavə edilmiş fayl hələ git status-da görünürsə, nə etməli?</div>
<div class="a">Fayl artıq commit olunub. `git rm --cached fayl` ilə index-dən çıxarıb yenidən commit etmək lazımdır.</div>

<div class="q">4. `git commit -am` nə edir, məhdudiyyəti nədir?</div>
<div class="a">Artıq izlənən faylları avtomatik stage edib commit edir. Məhdudiyyət: yeni (untracked) faylları stage etmir.</div>

<div class="q">5. `git commit --amend` nə vaxt TƏHLÜKƏLİDİR?</div>
<div class="a">Artıq push edilmiş commit-ə tətbiq etdikdə — hash dəyişir, başqalarının tarixçəsi ilə uyğunsuzluq yaranır.</div>

> **Data Platform & Engineering Bootcamp · DPE-01** · Git/GitHub/GitLab Modulu
