# docs/_src — PDF 생성용 원본

이 폴더의 HTML이 `docs/*.pdf`의 **유일한 원본**이다. PDF만 남기고 HTML을 버리면 다시 만들 수 없다.

## resume — 이력서

```bash
pip install weasyprint          # 69.0 기준
cd docs/_src/resume
python3 -c "from weasyprint import HTML; HTML('resume.html').write_pdf('Park_JongHyeok_Resume.pdf')"
```

- 결과물을 `docs/Park_JongHyeok_Resume.pdf` 로 복사
- 1페이지 미리보기는 `pdftoppm -r 110 -png -f 1 -l 1` 후 1241×1754로 리사이즈해 `media/docs/resume-preview.png` 로 저장
- 규격: A4 · 여백 20mm · 본문 9pt / line-height 1.8 · 강조색 `#a86b48`

## career — 경력기술서

```bash
cd docs/_src/career
python3 -c "from weasyprint import HTML; HTML('career.html').write_pdf('Park_JongHyeok_Career.pdf')"
```

- 결과물을 `docs/Park_JongHyeok_Career.pdf` 로 복사, 미리보기는 `media/docs/career-preview.png`
- 규격: A4 · 여백 18mm(하단 22mm) · 본문 8.5pt / line-height 1.62 · 핵심 역량 박스 `#f5f2ef` · 강조색 `#a86b48`
- 페이지 나눔은 `.proj.brk { break-before:page }` 로 트라하 블록에서 강제

## 통합본 (이력서 + 경력기술서)

**pypdf로 두 PDF를 병합하지 말 것.** 폰트 서브셋 이름이 충돌해 일부 뷰어에서 글자가 깨진다.
반드시 WeasyPrint 한 번의 렌더로 페이지를 합칠 것.

```python
from weasyprint import HTML
d1 = HTML('resume/resume.html').render()
d2 = HTML('career/career.html').render()
d1.copy(list(d1.pages) + list(d2.pages)).write_pdf('Park_JongHyeok_Resume_Career.pdf')
```

`@page` 규칙은 각 문서가 자기 것을 유지한다(이력서 20mm / 경력기술서 18mm·하단 22mm).
