---
title: Takım devre dışı bırakma
lang: tr
pdf: mcc-tool-disable-tr.pdf
---

# Takım devre dışı bırakma

<div class="document-tools" markdown="1">
[English](tool_disable_en.md) · **[PDF indir](downloads/mcc-tool-disable-tr.pdf)**
</div>

MCC, bir çizim için G kodunu **çizimdeki bütün takımlar için** oluşturur; yalnız seçili takımın kodunu çıkarmaz. Bir takımın işlemlerini geçici olarak yapmamak için G kodunu silmeniz veya yeniden oluşturmanız gerekmez: takımı **devre dışı bırakırsınız**, makine o takımın bölümünü çalışırken atlar.

## G kodunda nasıl görünür

Oluşturulan programda her takım ayrı bir blokla başlar. Bloğun ilk satırı bir atlama koşuludur: takımın devre dışı parametresi **0 değilse** NC, takımın bütün satırlarını atlayıp bir sonraki takımın bloğuna geçer.

```text
N01000 @121 R154 K0 K2000     ; R154 ≠ 0 ise N02000'e atla (takım 4)
...                           ; takım 4 işlemleri
N02000 @121 R156 K0 K3000     ; R156 ≠ 0 ise N03000'e atla (takım 6)
...                           ; takım 6 işlemleri
N03000 M17
```

B&R kontrollü makinelerde aynı mantık `IF R... <> 0` / `GOTO N...000` / `ENDIF` satırlarıyla yazılır.

**Parametre numarası = 150 + takım numarası.**

| Takım | Parametre | | Takım | Parametre |
|---|---|---|---|---|
| T1 | R151 | | T11 | R161 |
| T2 | R152 | | T12 | R162 |
| T3 | R153 | | T13 | R163 |
| T4 | R154 | | T14 | R164 |
| T5 | R155 | | T15 | R165 |
| T6 | R156 | | T16 | R166 |
| T7 | R157 | | T17 | R167 |
| T8 | R158 | | T18 | R168 |
| T9 | R159 | | T19 | R169 |
| T10 | R160 | | T20 | R170 |

## Yöntem 1 — Tool Disable kutusu

Takım ofset ekranında **Tool Disable** kutusu bulunan makinelerde (ör. CN779, CN784, DCM019B):

1. Takım ofset ekranını açın ve takımı seçin.
2. **Tool Disable** kutusunu işaretleyin ve kaydedin.
3. Takımı tekrar kullanmak için kutunun işaretini kaldırıp kaydedin.

## Yöntem 2 — R parametresi

Ekranında Tool Disable kutusu olmayan makinelerde (ör. CN770) takım, R parametreleriyle devre dışı bırakılır:

1. Servis menüsünden **R Parametreleri** sayfasını açın.
2. Takımın parametresine (150 + takım numarası) **1** yazın. Örnek: takım 4 için **R154 = 1**.
3. Takımı tekrar kullanmak için aynı parametreye **0** yazın.

R parametreleri kalıcıdır: makine kapatılıp açıldığında değer korunur. Değerler her program başlatılırken NC'ye yazılır.

## Dikkat edilecekler

- **Değişiklik bir sonraki program başlatmada geçerli olur.** Program çalışırken yapılan değişiklik, çalışan programı etkilemez.
- Devre dışı bırakılan takımın işlemleri parçada **yapılmaz**. Takım kullanıma döndüğünde parametreyi 0'a almayı / kutunun işaretini kaldırmayı unutmayın.
- Çizimde hiç kullanılmayan takım için parametre değerinin bir etkisi yoktur.
- Birden fazla takım aynı anda devre dışı bırakılabilir; her biri kendi bloğunu atlar.
