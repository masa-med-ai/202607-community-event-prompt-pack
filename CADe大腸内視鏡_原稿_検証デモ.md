# CADe（AI内視鏡）による大腸腺腫発見の向上 — 検証デモ用原稿

> このファイルは「pubmed-reference-verifier」スキルの動作デモ用原稿である。
> 本文中の引用4件のうち**1件のPMIDをあえて誤った数値**にしてある。
> スキルで検証すると、その1件が PubMed の書誌情報と一致せず検出される。

## 本文

大腸内視鏡検査では検出された病変の約4分の1が見逃されており、これが検査後大腸癌（interval cancer）の重要な原因となる。深層学習を用いたcomputer-aided detection（CADe）は、内視鏡画像をリアルタイムに解析して疑わしい病変を術者に提示することで、知覚的な見落としを補い腺腫発見率（ADR）を高める。GI-Geniusを用いた多施設無作為化試験では、CADe併用群のADRが54.8%と対照群の40.4%を有意に上回り（RR 1.30）、検査時間を延長させることなく1検査あたりの腺腫数も増加した{32371116}。同一患者を2回連続で観察するtandem試験では、AIにより腺腫見逃し率が32.4%から15.5%へと約半減し、とくに5mm以下や平坦・非ポリープ状病変での上乗せ効果が明確であった{35034117}。21試験・18,232例を統合したメタ解析でも、ADRは35.9%から44.0%へ上昇し（RR 1.24）、見逃し率は相対的に約55%減少することが示されている{37639719}。一方で、進行腫瘍（advanced neoplasia）の発見率向上には必ずしもつながらず{37639723}、非腫瘍性病変の不要な切除が微増する点には留意を要するが、総じてCADeは術者間の技量差を縮小し検査の質を標準化する有用な補助技術として臨床導入が進んでいる。

## References

1. Repici A, Badalamenti M, Maselli R, et al. Efficacy of Real-Time Computer-Aided Detection of Colorectal Neoplasia in a Randomized Trial. Gastroenterology. 2020;159(2):512-520.e7. doi:10.1053/j.gastro.2020.04.062. PMID: 32371116.
2. Wallace MB, Sharma P, Bhandari P, et al. Impact of Artificial Intelligence on Miss Rate of Colorectal Neoplasia. Gastroenterology. 2022;163(1):295-304.e5. doi:10.1053/j.gastro.2022.03.007. PMID: 35034117.
3. Hassan C, Spadaccini M, Mori Y, et al. Real-Time Computer-Aided Detection of Colorectal Neoplasia During Colonoscopy: A Systematic Review and Meta-analysis. Ann Intern Med. 2023;176(9):1209-1220. doi:10.7326/M22-3678. PMID: 37639719.
4. Mangas-Sanjuan C, de-Castro L, Cubiella J, et al. Role of Artificial Intelligence in Colonoscopy Detection of Advanced Neoplasias: A Randomized Trial. Ann Intern Med. 2023;176(9):1145-1152. doi:10.7326/M22-2619. PMID: 37639723.
