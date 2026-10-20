# MCC Manual

Service and operating guides for CNC machines with the MCC interface, in Turkish and English. The Turkish home page is `index.html`; the English home page is `home-en.html`. The header language switch opens the same topic in the other language.

MCC arayüzlü CNC makineleri için Türkçe ve İngilizce servis / kullanım rehberleri.

## 📖 Documentation

<https://hm-info.github.io/mcc/>

- [Türkçe — Takım devre dışı bırakma](https://hm-info.github.io/mcc/tool_disable_tr.html)
- [English — Disabling a tool](https://hm-info.github.io/mcc/tool_disable_en.html)

Every guide includes a PDF download. PDFs are generated automatically from the same Markdown during each build and are not committed to the source branch.

Documentation check: `pip install -r requirements.txt` then `mkdocs build --strict`. PDF generation requires Pango, HarfBuzz Subset and DejaVu fonts. Updates to `main` are published automatically to GitHub Pages.
