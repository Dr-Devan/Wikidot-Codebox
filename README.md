# Wikidot Codebox

A stable, unified, `lang`-based codebox system for Wikidot pages.

---

## Navigation

- [English](#english)
- [한국어](#한국어)
- [日本語](#日本語)

---

<a id="english"></a>

# English

## Overview

Wikidot Codebox provides reusable Wikidot snippets for displaying code with syntax highlighting, line numbers, copy support, theme switching, height expansion, line wrapping, custom scrollbars, and a multilingual safe encoder/decoder utility.

The stable implementation uses a single unified API:

```wikidot
[[include snippet:codebox
|title=Example Title
|lang=wikidot
]]

...

[[include snippet:codebox-end]]
```

---

## Stable Pages

The stable release consists of five Wikidot pages:

```text
snippet:codebox
snippet:codebox-base
snippet:codebox-end
snippet:codebox-end-base
snippet:codebox-decoder
```

Core pages:

```text
snippet:codebox
snippet:codebox-base
snippet:codebox-end
snippet:codebox-end-base
```

Utility page:

```text
snippet:codebox-decoder
```

---

## Quick Start

```wikidot
[[include snippet:codebox
|title=Wikidot Syntax Example
|lang=wikidot
]]

+ Heading

&#91;&#91;module CSS&#93;&#93;
.example {
  color: red;
}
&#91;&#91;/module&#93;&#93;

[[include snippet:codebox-end]]
```

---

## Public API

### `snippet:codebox`

Starts a codebox.

```wikidot
[[include snippet:codebox
|title=Example Title
|lang=wikidot
]]
```

Supported parameters:

```text
title
lang
```

### `title`

Display title shown in the codebox header.

Default:

```text
Code
```

### `lang`

Language identifier used for syntax highlighting.

Default:

```text
plaintext
```

Common values:

```text
plaintext
wikidot
html
css
javascript
typescript
python
bash
yaml
json
markup
```

---

### `snippet:codebox-end`

Ends a codebox.

```wikidot
[[include snippet:codebox-end]]
```

This page takes no public parameters.

---

## Page Responsibilities

### `snippet:codebox`

Public entry point for starting a codebox.

Responsibilities:

```text
- Receives title/lang parameters
- Applies fallback values
- Includes snippet:codebox-base
```

Stable code:

```wikidot
[[include snippet:codebox-base
|title{$title}=Code
|title={$title}
|title=Code
|lang{$lang}=plaintext
|lang={$lang}
|lang=plaintext
]]
```

---

### `snippet:codebox-base`

Internal base page for the visible codebox UI.

Responsibilities:

```text
- Codebox CSS
- Header layout
- Title and language display
- Dark/light theme toggle
- Height expand/collapse button
- Line wrap button
- Copy button
- Line number column
- Code output area
- Hidden textarea source container start
```

This page opens the source container:

```html
<textarea id="wd-source" style="display:none">
```

---

### `snippet:codebox-end`

Public entry point for ending a codebox.

Responsibilities:

```text
- Includes snippet:codebox-end-base
```

Stable code:

```wikidot
[[include snippet:codebox-end-base]]
```

---

### `snippet:codebox-end-base`

Internal base page for JavaScript behavior.

Responsibilities:

```text
- Closes the source textarea
- Loads Prism.js
- Normalizes language aliases
- Registers custom Wikidot grammar
- Renders highlighted code
- Generates line numbers
- Handles copy behavior
- Shows copy toast
- Preserves scroll position
- Applies dark/light theme
- Handles height expand/collapse
- Handles line wrapping
- Generates wrapped-line-number spacers
- Uses ResizeObserver to recalculate wrapped line numbers
```

This page closes the source container:

```html
</textarea>
```

---

### `snippet:codebox-decoder`

Utility page for generating safe Codebox payloads.

Responsibilities:

```text
- Wikidot [[html]] based encoder/decoder web app
- Stable snippet:codebox include generation
- UI language selection
- Include title input
- Language selection
- Source code input
- HTML-sensitive character encoding
- Wikidot bracket encoding for lang=wikidot
- Entity decoding
- Copy output
- Copy encoded body
- Dark/light theme toggle
- Scroll jump prevention
```

Supported UI languages:

```text
English
한국어
日本語
Français
Deutsch
中文
Español
```

---

## Language Normalization

Runtime language aliases:

```text
wd       → wikidot
wiki     → wikidot
html     → markup
xml      → markup
svg      → markup
js       → javascript
ts       → typescript
sh       → bash
shell    → bash
py       → python
rb       → ruby
yml      → yaml
```

The displayed language label is based on the provided `lang` value.

---

## Source Container

All codeboxes use the same source container:

```html
<textarea id="wd-source" style="display:none">
```

Because source code is stored inside a `textarea`, some characters must be safely encoded before being inserted into the page.

---

## Safe Input Rules

### Base Encoding

Recommended base encoding:

```text
&  → &amp;
<  → &lt;
>  → &gt;
```

Use `snippet:codebox-decoder` to generate safe payloads automatically.

---

## HTML Input

HTML source should be entity-encoded before being placed inside a codebox.

```wikidot
[[include snippet:codebox
|title=HTML Textarea Example
|lang=html
]]

&lt;form class="memo-form"&gt;
  &lt;label for="memo"&gt;Memo&lt;/label&gt;
  &lt;textarea id="memo" name="memo" rows="4" cols="40"&gt;
hello
  &lt;/textarea&gt;
  &lt;button type="submit"&gt;Submit&lt;/button&gt;
&lt;/form&gt;

[[include snippet:codebox-end]]
```

Rendered and copied source:

```html
<form class="memo-form">
  <label for="memo">Memo</label>
  <textarea id="memo" name="memo" rows="4" cols="40">
hello
  </textarea>
  <button type="submit">Submit</button>
</form>
```

---

## Wikidot Input

Wikidot syntax that should not execute must be escaped.

Recommended bracket encoding:

```text
[  → &#91;
]  → &#93;
```

Example:

```wikidot
[[include snippet:codebox
|title=Wikidot Syntax Example
|lang=wikidot
]]

+ Heading

&#91;&#91;module CSS&#93;&#93;
.example {
  color: red;
}
&#91;&#91;/module&#93;&#93;

[[include snippet:codebox-end]]
```

Rendered and copied source:

```wikidot
+ Heading

[[module CSS]]
.example {
  color: red;
}
[[/module]]
```

---

## Examples

### CSS Example

```wikidot
[[include snippet:codebox
|title=CSS Example
|lang=css
]]

.card {
  display: grid;
  gap: 1rem;
  border-radius: 12px;
}

[[include snippet:codebox-end]]
```

### JavaScript Example

```wikidot
[[include snippet:codebox
|title=JavaScript Example
|lang=javascript
]]

function hello(name) {
  console.log("Hello, " + name);
}

[[include snippet:codebox-end]]
```

---

## Features

```text
- Unified lang-based codebox
- title/lang fallback
- Prism.js syntax highlighting
- Custom Wikidot grammar
- Dark/light theme toggle
- Theme persistence via localStorage
- Copy button
- Copy toast
- Scroll restoration on copy, theme toggle, expand, and wrap
- Line numbers
- Expand/collapse height
- Fixed px-based expanded height
- Custom scrollbar styling
- Line wrap toggle
- Wrapped line-number spacer rendering
- ResizeObserver-based wrapped line-number recalculation
- Hidden textarea source container
- Safe HTML/Wikidot payload workflow
- Multilingual decoder utility
```

---

## Theme Behavior

Supported themes:

```text
dark
light
```

Default theme:

```text
dark
```

Theme selection is persisted with `localStorage`.

---

## Height Expansion

Default collapsed height:

```text
520px
```

Expanded height:

```text
960px
```

State attributes:

```html
data-expanded="false"
```

```html
data-expanded="true"
```

Expand state is not persisted.

---

## Line Wrapping

Default:

```text
Wrap OFF
```

State attributes:

```html
data-wrap="false"
```

```html
data-wrap="true"
```

Wrap state is not persisted.

### Wrap OFF

```text
- white-space: pre
- horizontal scrollbar enabled
- line numbers match original source lines
```

### Wrap ON

```text
- white-space: pre-wrap
- overflow-wrap: anywhere
- word-break: normal
- horizontal overflow hidden
- line numbers remain visible
- wrapped visual lines receive blank line-number spacer rows
```

If original line 1 wraps into three visual rows and original line 2 follows it, line numbers are rendered conceptually as:

```text
1


2
```

Equivalent conceptual DOM:

```html
<span>1</span>
<span></span>
<span></span>
<span>2</span>
```

---

## Copy Behavior

Copy always uses the original normalized source from the hidden `textarea`.

Visual wrapping, syntax highlighting, and rendered DOM do not affect copied text.

Copy flow:

```text
source.value → normalizeSource() → clipboard
```

---

## Scrollbar Styling

Standard scrollbar styling:

```css
scrollbar-width: thin;
scrollbar-color: var(--wd-scrollbar-thumb) var(--wd-scrollbar-track);
scrollbar-gutter: stable;
```

WebKit scrollbar styling:

```css
::-webkit-scrollbar
::-webkit-scrollbar-track
::-webkit-scrollbar-thumb
::-webkit-scrollbar-corner
```

Both horizontal and vertical scrollbars are styled.

---

## Custom Wikidot Grammar

The stable version includes a custom Prism grammar for Wikidot syntax.

Supported token groups:

```text
- comments
- code blocks
- html blocks
- headings
- tables
- lists
- horizontal rules
- triple links
- external links
- bare URLs
- closing blocks
- include blocks
- module blocks
- user blocks
- tab blocks
- generic blocks
- template variables
- include variables
- color syntax
- inline formatting
- footnotes
- bibliography blocks
```

Aliases:

```text
wikidot
wd
wiki
```

---

## Decoder Utility Workflow

Recommended workflow:

```text
1. Open snippet:codebox-decoder.
2. Select UI language.
3. Enter include title.
4. Select code language.
5. Paste raw source code.
6. Generate output.
7. Copy output.
8. Paste into a Wikidot page.
```

Generated output format:

```wikidot
[[include snippet:codebox
|title=...
|lang=...
]]

...

[[include snippet:codebox-end]]
```

---

## Deprecated / Removed Pages

The following pages are not part of the stable spec:

```text
snippet:codebox-dispatch
snippet:codebox-end-dispatch
snippet:codebox-text
snippet:codebox-text-end
snippet:codebox-html
snippet:codebox-html-end
```

They can be deleted if there are no references to them.

---

## Recommended Repository Structure

```text
/
├─ README.md
├─ LICENSE
├─ LICENSE.docs
├─ snippets/
│  ├─ snippet-codebox.wikidot
│  ├─ snippet-codebox-base.wikidot
│  ├─ snippet-codebox-end.wikidot
│  ├─ snippet-codebox-end-base.wikidot
│  └─ snippet-codebox-decoder.wikidot
└─ examples/
   ├─ wikidot-example.wikidot
   ├─ html-example.wikidot
   ├─ css-example.wikidot
   └─ javascript-example.wikidot
```

---

## License

Code in this repository, including Wikidot snippets, JavaScript, and CSS, is licensed under the GNU General Public License v3.0 or later.

Documentation, explanatory text, and non-code examples are licensed under the Creative Commons Attribution-ShareAlike 4.0 International License.

SPDX identifiers:

```text
Code: GPL-3.0-or-later
Documentation: CC-BY-SA-4.0
```

---

## Release Notes

### Stable Release

Pages:

```text
snippet:codebox
snippet:codebox-base
snippet:codebox-end
snippet:codebox-end-base
snippet:codebox-decoder
```

Highlights:

```text
- Unified lang-based codebox implementation
- Removed code and trimIncludeEscape parameters
- Added title/lang fallback
- Added custom Wikidot syntax grammar
- Added dark/light theme toggle
- Added height expand/collapse
- Added line wrap toggle
- Added wrapped line-number spacer rendering
- Added custom scrollbar styling
- Added multilingual safe encoder/decoder utility
- Stabilized HTML and Wikidot safe payload workflow
```

---

## Status

Stable.

---

<a id="한국어"></a>

# 한국어

## 개요

Wikidot Codebox는 Wikidot 페이지에서 코드를 안정적으로 표시하기 위한 재사용 가능한 snippet 기반 code viewer입니다.

Syntax highlighting, line number, copy button, theme switching, height expansion, line wrapping, custom scrollbar, multilingual safe encoder/decoder utility를 제공합니다.

Stable 구현은 하나의 통합 API를 사용합니다.

```wikidot
[[include snippet:codebox
|title=Example Title
|lang=wikidot
]]

...

[[include snippet:codebox-end]]
```

---

## Stable 페이지

Stable 릴리스는 다음 다섯 개의 Wikidot 페이지로 구성됩니다.

```text
snippet:codebox
snippet:codebox-base
snippet:codebox-end
snippet:codebox-end-base
snippet:codebox-decoder
```

Core 페이지:

```text
snippet:codebox
snippet:codebox-base
snippet:codebox-end
snippet:codebox-end-base
```

Utility 페이지:

```text
snippet:codebox-decoder
```

---

## 빠른 시작

```wikidot
[[include snippet:codebox
|title=Wikidot Syntax Example
|lang=wikidot
]]

+ Heading

&#91;&#91;module CSS&#93;&#93;
.example {
  color: red;
}
&#91;&#91;/module&#93;&#93;

[[include snippet:codebox-end]]
```

---

## 공개 API

### `snippet:codebox`

Codebox를 시작합니다.

```wikidot
[[include snippet:codebox
|title=Example Title
|lang=wikidot
]]
```

지원 파라미터:

```text
title
lang
```

### `title`

Codebox 헤더에 표시되는 제목입니다.

기본값:

```text
Code
```

### `lang`

Syntax highlighting에 사용할 언어 식별자입니다.

기본값:

```text
plaintext
```

주요 값:

```text
plaintext
wikidot
html
css
javascript
typescript
python
bash
yaml
json
markup
```

---

### `snippet:codebox-end`

Codebox를 종료합니다.

```wikidot
[[include snippet:codebox-end]]
```

공개 파라미터는 없습니다.

---

## 페이지 역할

### `snippet:codebox`

Codebox 시작용 공개 entry 페이지입니다.

역할:

```text
- title/lang 파라미터 수신
- 기본값 보정
- snippet:codebox-base include
```

Stable 코드:

```wikidot
[[include snippet:codebox-base
|title{$title}=Code
|title={$title}
|title=Code
|lang{$lang}=plaintext
|lang={$lang}
|lang=plaintext
]]
```

---

### `snippet:codebox-base`

실제 Codebox UI 시작부입니다.

역할:

```text
- Codebox CSS
- Header layout
- Title/lang 표시
- Dark/light theme toggle
- Height expand/collapse button
- Line wrap button
- Copy button
- Line number column
- Code output area
- Hidden textarea source container 시작
```

이 페이지는 source container를 엽니다.

```html
<textarea id="wd-source" style="display:none">
```

---

### `snippet:codebox-end`

Codebox 종료용 공개 entry 페이지입니다.

역할:

```text
- snippet:codebox-end-base include
```

Stable 코드:

```wikidot
[[include snippet:codebox-end-base]]
```

---

### `snippet:codebox-end-base`

실제 JavaScript 동작을 담당하는 종료부입니다.

역할:

```text
- source textarea 종료
- Prism.js 로드
- 언어 alias 정규화
- Wikidot custom grammar 등록
- highlighted code 렌더링
- line number 생성
- copy 처리
- copy toast 표시
- scroll position 보존
- dark/light theme 적용
- height expand/collapse 처리
- line wrapping 처리
- wrapped-line-number spacer 생성
- ResizeObserver로 wrapped line number 재계산
```

이 페이지는 source container를 닫습니다.

```html
</textarea>
```

---

### `snippet:codebox-decoder`

안전한 Codebox payload를 생성하는 utility 페이지입니다.

역할:

```text
- Wikidot [[html]] 기반 encoder/decoder web app
- Stable snippet:codebox include 생성
- UI 언어 선택
- Include title 입력
- Language 선택
- Source code 입력
- HTML-sensitive character 인코딩
- lang=wikidot일 때 Wikidot bracket 인코딩
- Entity decoding
- Copy output
- Copy encoded body
- Dark/light theme toggle
- Scroll jump 방지
```

지원 UI 언어:

```text
English
한국어
日本語
Français
Deutsch
中文
Español
```

---

## 언어 정규화

Runtime 언어 alias:

```text
wd       → wikidot
wiki     → wikidot
html     → markup
xml      → markup
svg      → markup
js       → javascript
ts       → typescript
sh       → bash
shell    → bash
py       → python
rb       → ruby
yml      → yaml
```

표시되는 언어 label은 사용자가 입력한 `lang` 값을 기준으로 합니다.

---

## Source Container

모든 Codebox는 동일한 source container를 사용합니다.

```html
<textarea id="wd-source" style="display:none">
```

소스 코드가 `textarea` 안에 저장되기 때문에, 페이지에 삽입하기 전에 일부 문자는 안전하게 인코딩해야 합니다.

---

## 안전 입력 규칙

### 기본 인코딩

권장 기본 인코딩:

```text
&  → &amp;
<  → &lt;
>  → &gt;
```

안전한 payload는 `snippet:codebox-decoder`로 자동 생성하는 것을 권장합니다.

---

## HTML 입력

HTML source는 Codebox 안에 넣기 전에 entity-encoded 상태여야 합니다.

```wikidot
[[include snippet:codebox
|title=HTML Textarea Example
|lang=html
]]

&lt;form class="memo-form"&gt;
  &lt;label for="memo"&gt;Memo&lt;/label&gt;
  &lt;textarea id="memo" name="memo" rows="4" cols="40"&gt;
hello
  &lt;/textarea&gt;
  &lt;button type="submit"&gt;Submit&lt;/button&gt;
&lt;/form&gt;

[[include snippet:codebox-end]]
```

렌더링 및 복사되는 실제 source:

```html
<form class="memo-form">
  <label for="memo">Memo</label>
  <textarea id="memo" name="memo" rows="4" cols="40">
hello
  </textarea>
  <button type="submit">Submit</button>
</form>
```

---

## Wikidot 입력

실행되면 안 되는 Wikidot 문법은 escape해야 합니다.

권장 bracket 인코딩:

```text
[  → &#91;
]  → &#93;
```

예시:

```wikidot
[[include snippet:codebox
|title=Wikidot Syntax Example
|lang=wikidot
]]

+ Heading

&#91;&#91;module CSS&#93;&#93;
.example {
  color: red;
}
&#91;&#91;/module&#93;&#93;

[[include snippet:codebox-end]]
```

렌더링 및 복사되는 실제 source:

```wikidot
+ Heading

[[module CSS]]
.example {
  color: red;
}
[[/module]]
```

---

## 예제

### CSS 예제

```wikidot
[[include snippet:codebox
|title=CSS Example
|lang=css
]]

.card {
  display: grid;
  gap: 1rem;
  border-radius: 12px;
}

[[include snippet:codebox-end]]
```

### JavaScript 예제

```wikidot
[[include snippet:codebox
|title=JavaScript Example
|lang=javascript
]]

function hello(name) {
  console.log("Hello, " + name);
}

[[include snippet:codebox-end]]
```

---

## 기능

```text
- lang 기반 unified codebox
- title/lang fallback
- Prism.js syntax highlighting
- Custom Wikidot grammar
- Dark/light theme toggle
- localStorage 기반 theme persistence
- Copy button
- Copy toast
- Copy, theme toggle, expand, wrap 시 scroll restoration
- Line numbers
- Expand/collapse height
- px 기반 fixed expanded height
- Custom scrollbar styling
- Line wrap toggle
- Wrapped line-number spacer rendering
- ResizeObserver 기반 wrapped line-number recalculation
- Hidden textarea source container
- Safe HTML/Wikidot payload workflow
- Multilingual decoder utility
```

---

## Theme 동작

지원 theme:

```text
dark
light
```

기본 theme:

```text
dark
```

Theme 선택은 `localStorage`에 저장됩니다.

---

## Height Expansion

기본 collapsed height:

```text
520px
```

Expanded height:

```text
960px
```

상태 attribute:

```html
data-expanded="false"
```

```html
data-expanded="true"
```

Expand 상태는 저장하지 않습니다.

---

## Line Wrapping

기본값:

```text
Wrap OFF
```

상태 attribute:

```html
data-wrap="false"
```

```html
data-wrap="true"
```

Wrap 상태는 저장하지 않습니다.

### Wrap OFF

```text
- white-space: pre
- horizontal scrollbar enabled
- line numbers match original source lines
```

### Wrap ON

```text
- white-space: pre-wrap
- overflow-wrap: anywhere
- word-break: normal
- horizontal overflow hidden
- line numbers remain visible
- wrapped visual lines receive blank line-number spacer rows
```

원본 1번 줄이 화면상 3줄로 wrap되고 그 다음에 2번 줄이 온다면, 줄 번호는 개념적으로 다음처럼 렌더링됩니다.

```text
1


2
```

개념적 DOM:

```html
<span>1</span>
<span></span>
<span></span>
<span>2</span>
```

---

## Copy 동작

Copy는 항상 hidden `textarea`의 원본 normalized source를 사용합니다.

Visual wrapping, syntax highlighting, rendered DOM은 복사 결과에 영향을 주지 않습니다.

Copy flow:

```text
source.value → normalizeSource() → clipboard
```

---

## Scrollbar Styling

표준 scrollbar styling:

```css
scrollbar-width: thin;
scrollbar-color: var(--wd-scrollbar-thumb) var(--wd-scrollbar-track);
scrollbar-gutter: stable;
```

WebKit scrollbar styling:

```css
::-webkit-scrollbar
::-webkit-scrollbar-track
::-webkit-scrollbar-thumb
::-webkit-scrollbar-corner
```

가로/세로 scrollbar 모두 스타일링합니다.

---

## Custom Wikidot Grammar

Stable 버전은 Wikidot syntax용 custom Prism grammar를 포함합니다.

지원 token group:

```text
- comments
- code blocks
- html blocks
- headings
- tables
- lists
- horizontal rules
- triple links
- external links
- bare URLs
- closing blocks
- include blocks
- module blocks
- user blocks
- tab blocks
- generic blocks
- template variables
- include variables
- color syntax
- inline formatting
- footnotes
- bibliography blocks
```

Aliases:

```text
wikidot
wd
wiki
```

---

## Decoder 유틸리티 Workflow

권장 workflow:

```text
1. snippet:codebox-decoder를 엽니다.
2. UI 언어를 선택합니다.
3. Include title을 입력합니다.
4. Code language를 선택합니다.
5. Raw source code를 붙여넣습니다.
6. Generate output을 실행합니다.
7. Copy output을 누릅니다.
8. Wikidot 페이지에 붙여넣습니다.
```

생성되는 output 형식:

```wikidot
[[include snippet:codebox
|title=...
|lang=...
]]

...

[[include snippet:codebox-end]]
```

---

## Deprecated / Removed 페이지

다음 페이지들은 stable 사양에 포함되지 않습니다.

```text
snippet:codebox-dispatch
snippet:codebox-end-dispatch
snippet:codebox-text
snippet:codebox-text-end
snippet:codebox-html
snippet:codebox-html-end
```

참조가 없다면 삭제할 수 있습니다.

---

## 권장 Repository 구조

```text
/
├─ README.md
├─ LICENSE
├─ LICENSE.docs
├─ snippets/
│  ├─ snippet-codebox.wikidot
│  ├─ snippet-codebox-base.wikidot
│  ├─ snippet-codebox-end.wikidot
│  ├─ snippet-codebox-end-base.wikidot
│  └─ snippet-codebox-decoder.wikidot
└─ examples/
   ├─ wikidot-example.wikidot
   ├─ html-example.wikidot
   ├─ css-example.wikidot
   └─ javascript-example.wikidot
```

---

## 라이선스

이 저장소의 코드, 즉 Wikidot snippets, JavaScript, CSS는 GNU General Public License v3.0 or later로 배포됩니다.

문서, 설명 텍스트, non-code 예제는 Creative Commons Attribution-ShareAlike 4.0 International License로 배포됩니다.

SPDX identifiers:

```text
Code: GPL-3.0-or-later
Documentation: CC-BY-SA-4.0
```

---

## Release Notes

### Stable Release

Pages:

```text
snippet:codebox
snippet:codebox-base
snippet:codebox-end
snippet:codebox-end-base
snippet:codebox-decoder
```

Highlights:

```text
- Unified lang-based codebox implementation
- Removed code and trimIncludeEscape parameters
- Added title/lang fallback
- Added custom Wikidot syntax grammar
- Added dark/light theme toggle
- Added height expand/collapse
- Added line wrap toggle
- Added wrapped line-number spacer rendering
- Added custom scrollbar styling
- Added multilingual safe encoder/decoder utility
- Stabilized HTML and Wikidot safe payload workflow
```

---

## 상태

Stable.

---

<a id="日本語"></a>

# 日本語

## 概要

Wikidot Codebox は、Wikidot ページでコードを安定して表示するための再利用可能な snippet ベースの code viewer です。

Syntax highlighting、line number、copy button、theme switching、height expansion、line wrapping、custom scrollbar、多言語 safe encoder/decoder utility を提供します。

Stable 実装は単一の統合 API を使用します。

```wikidot
[[include snippet:codebox
|title=Example Title
|lang=wikidot
]]

...

[[include snippet:codebox-end]]
```

---

## Stable ページ

Stable リリースは次の 5 つの Wikidot ページで構成されます。

```text
snippet:codebox
snippet:codebox-base
snippet:codebox-end
snippet:codebox-end-base
snippet:codebox-decoder
```

Core ページ:

```text
snippet:codebox
snippet:codebox-base
snippet:codebox-end
snippet:codebox-end-base
```

Utility ページ:

```text
snippet:codebox-decoder
```

---

## クイックスタート

```wikidot
[[include snippet:codebox
|title=Wikidot Syntax Example
|lang=wikidot
]]

+ Heading

&#91;&#91;module CSS&#93;&#93;
.example {
  color: red;
}
&#91;&#91;/module&#93;&#93;

[[include snippet:codebox-end]]
```

---

## 公開 API

### `snippet:codebox`

Codebox を開始します。

```wikidot
[[include snippet:codebox
|title=Example Title
|lang=wikidot
]]
```

対応パラメータ:

```text
title
lang
```

### `title`

Codebox ヘッダーに表示されるタイトルです。

既定値:

```text
Code
```

### `lang`

Syntax highlighting に使用する言語識別子です。

既定値:

```text
plaintext
```

主な値:

```text
plaintext
wikidot
html
css
javascript
typescript
python
bash
yaml
json
markup
```

---

### `snippet:codebox-end`

Codebox を終了します。

```wikidot
[[include snippet:codebox-end]]
```

公開パラメータはありません。

---

## ページの役割

### `snippet:codebox`

Codebox 開始用の公開 entry ページです。

役割:

```text
- title/lang パラメータを受け取る
- 既定値を補正する
- snippet:codebox-base を include する
```

Stable コード:

```wikidot
[[include snippet:codebox-base
|title{$title}=Code
|title={$title}
|title=Code
|lang{$lang}=plaintext
|lang={$lang}
|lang=plaintext
]]
```

---

### `snippet:codebox-base`

実際の Codebox UI 開始部です。

役割:

```text
- Codebox CSS
- Header layout
- Title/lang 表示
- Dark/light theme toggle
- Height expand/collapse button
- Line wrap button
- Copy button
- Line number column
- Code output area
- Hidden textarea source container 開始
```

このページは source container を開きます。

```html
<textarea id="wd-source" style="display:none">
```

---

### `snippet:codebox-end`

Codebox 終了用の公開 entry ページです。

役割:

```text
- snippet:codebox-end-base を include する
```

Stable コード:

```wikidot
[[include snippet:codebox-end-base]]
```

---

### `snippet:codebox-end-base`

実際の JavaScript 動作を担当する終了部です。

役割:

```text
- source textarea を閉じる
- Prism.js をロードする
- 言語 alias を正規化する
- Wikidot custom grammar を登録する
- highlighted code をレンダリングする
- line number を生成する
- copy を処理する
- copy toast を表示する
- scroll position を保持する
- dark/light theme を適用する
- height expand/collapse を処理する
- line wrapping を処理する
- wrapped-line-number spacer を生成する
- ResizeObserver で wrapped line number を再計算する
```

このページは source container を閉じます。

```html
</textarea>
```

---

### `snippet:codebox-decoder`

安全な Codebox payload を生成する utility ページです。

役割:

```text
- Wikidot [[html]] ベースの encoder/decoder web app
- Stable snippet:codebox include を生成
- UI 言語選択
- Include title 入力
- Language 選択
- Source code 入力
- HTML-sensitive character のエンコード
- lang=wikidot の場合の Wikidot bracket エンコード
- Entity decoding
- Copy output
- Copy encoded body
- Dark/light theme toggle
- Scroll jump 防止
```

対応 UI 言語:

```text
English
한국어
日本語
Français
Deutsch
中文
Español
```

---

## 言語正規化

Runtime language aliases:

```text
wd       → wikidot
wiki     → wikidot
html     → markup
xml      → markup
svg      → markup
js       → javascript
ts       → typescript
sh       → bash
shell    → bash
py       → python
rb       → ruby
yml      → yaml
```

表示される言語 label は、指定された `lang` 値を基準にします。

---

## Source Container

すべての Codebox は同じ source container を使用します。

```html
<textarea id="wd-source" style="display:none">
```

ソースコードは `textarea` 内に保存されるため、ページに挿入する前に一部の文字を安全にエンコードする必要があります。

---

## 安全な入力ルール

### 基本エンコード

推奨される基本エンコード:

```text
&  → &amp;
<  → &lt;
>  → &gt;
```

安全な payload は `snippet:codebox-decoder` で自動生成することを推奨します。

---

## HTML 入力

HTML source は Codebox に入れる前に entity-encoded 状態にしてください。

```wikidot
[[include snippet:codebox
|title=HTML Textarea Example
|lang=html
]]

&lt;form class="memo-form"&gt;
  &lt;label for="memo"&gt;Memo&lt;/label&gt;
  &lt;textarea id="memo" name="memo" rows="4" cols="40"&gt;
hello
  &lt;/textarea&gt;
  &lt;button type="submit"&gt;Submit&lt;/button&gt;
&lt;/form&gt;

[[include snippet:codebox-end]]
```

レンダリングおよびコピーされる実際の source:

```html
<form class="memo-form">
  <label for="memo">Memo</label>
  <textarea id="memo" name="memo" rows="4" cols="40">
hello
  </textarea>
  <button type="submit">Submit</button>
</form>
```

---

## Wikidot 入力

実行されてはいけない Wikidot 構文は escape してください。

推奨 bracket encoding:

```text
[  → &#91;
]  → &#93;
```

例:

```wikidot
[[include snippet:codebox
|title=Wikidot Syntax Example
|lang=wikidot
]]

+ Heading

&#91;&#91;module CSS&#93;&#93;
.example {
  color: red;
}
&#91;&#91;/module&#93;&#93;

[[include snippet:codebox-end]]
```

レンダリングおよびコピーされる実際の source:

```wikidot
+ Heading

[[module CSS]]
.example {
  color: red;
}
[[/module]]
```

---

## 例

### CSS 例

```wikidot
[[include snippet:codebox
|title=CSS Example
|lang=css
]]

.card {
  display: grid;
  gap: 1rem;
  border-radius: 12px;
}

[[include snippet:codebox-end]]
```

### JavaScript 例

```wikidot
[[include snippet:codebox
|title=JavaScript Example
|lang=javascript
]]

function hello(name) {
  console.log("Hello, " + name);
}

[[include snippet:codebox-end]]
```

---

## 機能

```text
- lang ベースの unified codebox
- title/lang fallback
- Prism.js syntax highlighting
- Custom Wikidot grammar
- Dark/light theme toggle
- localStorage による theme persistence
- Copy button
- Copy toast
- Copy, theme toggle, expand, wrap 時の scroll restoration
- Line numbers
- Expand/collapse height
- px ベースの fixed expanded height
- Custom scrollbar styling
- Line wrap toggle
- Wrapped line-number spacer rendering
- ResizeObserver ベースの wrapped line-number recalculation
- Hidden textarea source container
- Safe HTML/Wikidot payload workflow
- Multilingual decoder utility
```

---

## Theme 動作

対応 theme:

```text
dark
light
```

既定 theme:

```text
dark
```

Theme 選択は `localStorage` に保存されます。

---

## Height Expansion

既定 collapsed height:

```text
520px
```

Expanded height:

```text
960px
```

状態 attribute:

```html
data-expanded="false"
```

```html
data-expanded="true"
```

Expand 状態は保存されません。

---

## Line Wrapping

既定値:

```text
Wrap OFF
```

状態 attribute:

```html
data-wrap="false"
```

```html
data-wrap="true"
```

Wrap 状態は保存されません。

### Wrap OFF

```text
- white-space: pre
- horizontal scrollbar enabled
- line numbers match original source lines
```

### Wrap ON

```text
- white-space: pre-wrap
- overflow-wrap: anywhere
- word-break: normal
- horizontal overflow hidden
- line numbers remain visible
- wrapped visual lines receive blank line-number spacer rows
```

元の 1 行目が画面上で 3 行に wrap され、その次に 2 行目が続く場合、行番号は概念的に次のようにレンダリングされます。

```text
1


2
```

概念的な DOM:

```html
<span>1</span>
<span></span>
<span></span>
<span>2</span>
```

---

## Copy 動作

Copy は常に hidden `textarea` の normalized source を使用します。

Visual wrapping、syntax highlighting、rendered DOM はコピー結果に影響しません。

Copy flow:

```text
source.value → normalizeSource() → clipboard
```

---

## Scrollbar Styling

標準 scrollbar styling:

```css
scrollbar-width: thin;
scrollbar-color: var(--wd-scrollbar-thumb) var(--wd-scrollbar-track);
scrollbar-gutter: stable;
```

WebKit scrollbar styling:

```css
::-webkit-scrollbar
::-webkit-scrollbar-track
::-webkit-scrollbar-thumb
::-webkit-scrollbar-corner
```

横方向・縦方向の scrollbar を両方スタイリングします。

---

## Custom Wikidot Grammar

Stable 版には Wikidot syntax 用 custom Prism grammar が含まれています。

対応 token group:

```text
- comments
- code blocks
- html blocks
- headings
- tables
- lists
- horizontal rules
- triple links
- external links
- bare URLs
- closing blocks
- include blocks
- module blocks
- user blocks
- tab blocks
- generic blocks
- template variables
- include variables
- color syntax
- inline formatting
- footnotes
- bibliography blocks
```

Aliases:

```text
wikidot
wd
wiki
```

---

## Decoder ユーティリティ Workflow

推奨 workflow:

```text
1. snippet:codebox-decoder を開きます。
2. UI 言語を選択します。
3. Include title を入力します。
4. Code language を選択します。
5. Raw source code を貼り付けます。
6. Generate output を実行します。
7. Copy output を押します。
8. Wikidot ページに貼り付けます。
```

生成される output 形式:

```wikidot
[[include snippet:codebox
|title=...
|lang=...
]]

...

[[include snippet:codebox-end]]
```

---

## Deprecated / Removed ページ

次のページは stable 仕様には含まれません。

```text
snippet:codebox-dispatch
snippet:codebox-end-dispatch
snippet:codebox-text
snippet:codebox-text-end
snippet:codebox-html
snippet:codebox-html-end
```

参照がなければ削除できます。

---

## 推奨 Repository 構成

```text
/
├─ README.md
├─ LICENSE
├─ LICENSE.docs
├─ snippets/
│  ├─ snippet-codebox.wikidot
│  ├─ snippet-codebox-base.wikidot
│  ├─ snippet-codebox-end.wikidot
│  ├─ snippet-codebox-end-base.wikidot
│  └─ snippet-codebox-decoder.wikidot
└─ examples/
   ├─ wikidot-example.wikidot
   ├─ html-example.wikidot
   ├─ css-example.wikidot
   └─ javascript-example.wikidot
```

---

## ライセンス

このリポジトリのコード、つまり Wikidot snippets、JavaScript、CSS は GNU General Public License v3.0 or later でライセンスされます。

ドキュメント、説明テキスト、non-code example は Creative Commons Attribution-ShareAlike 4.0 International License でライセンスされます。

SPDX identifiers:

```text
Code: GPL-3.0-or-later
Documentation: CC-BY-SA-4.0
```

---

## Release Notes

### Stable Release

Pages:

```text
snippet:codebox
snippet:codebox-base
snippet:codebox-end
snippet:codebox-end-base
snippet:codebox-decoder
```

Highlights:

```text
- Unified lang-based codebox implementation
- Removed code and trimIncludeEscape parameters
- Added title/lang fallback
- Added custom Wikidot syntax grammar
- Added dark/light theme toggle
- Added height expand/collapse
- Added line wrap toggle
- Added wrapped line-number spacer rendering
- Added custom scrollbar styling
- Added multilingual safe encoder/decoder utility
- Stabilized HTML and Wikidot safe payload workflow
```

---

## 状態

Stable.
