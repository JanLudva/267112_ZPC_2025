Návod: zkopíruj svůj obrázek(y) do složky `static/images` tak, aby je Hugo při sestavení naservíroval jako `/images/<filename>`.

Příklad 1 — zkopírovat všechny PNG/JPG z tvé složky (pokud chceš zkopírovat jen jeden soubor, nahraď masku konkrétním názvem souboru):

# V PowerShellu spusť (přizpůsob cestu pokud je jiná):
New-Item -ItemType Directory -Path 'c:\Hugo\my-site\static\images' -Force
Copy-Item -Path 'C:\Users\janlu\OneDrive\Plocha\škola\3 semestr\ZPC\web\kód\*.png' -Destination 'c:\Hugo\my-site\static\images' -Force
Copy-Item -Path 'C:\Users\janlu\OneDrive\Plocha\škola\3 semestr\ZPC\web\kód\*.jpg' -Destination 'c:\Hugo\my-site\static\images' -Force

# Nebo zkopírovat vše najednou (png,jpg,webp):
Copy-Item -Path 'C:\Users\janlu\OneDrive\Plocha\škola\3 semestr\ZPC\web\kód\*' -Destination 'c:\Hugo\my-site\static\images' -Recurse -Force

Příklad 2 — zkopírovat konkrétní soubor a přejmenovat ho na `kod.png` (pokud chceš zachovat název, změň cílové jméno):
Copy-Item -Path 'C:\Users\janlu\OneDrive\Plocha\škola\3 semestr\ZPC\web\kód\nazev_souboru.png' -Destination 'c:\Hugo\my-site\static\images\\kod.png' -Force

Po zkopírování spusť v kořenovém adresáři webu Hugo server pro kontrolu:
cd 'c:\Hugo\my-site'
hugo server -D

Otevři http://localhost:1313 a přejdi na stránku projektu 1 — obrázek by se měl zobrazit tam, kde je v souboru `project1.md` odkaz na `/images/kod.png`.

Poznámka o názvech souborů:
- Doporučuji používat web‑bezpečné názvy souborů (bez diakritiky, bez mezer), např. `kod.png` místo `kód.png`.
- Pokud chceš přejmenovat soubor v `static/images` pomocí PowerShellu, můžeš spustit (přejmenuje `kód.png` na `kod.png`):
Rename-Item -Path 'c:\Hugo\my-site\static\images\kód.png' -NewName 'kod.png' -Force

Pokud nechceš přejmenovávat, ujisti se, že v Markdownu odkazuješ přesně na aktuální název souboru (včetně diakritiky), např. `![Obrázek kódu](/images/kód.png)`.
